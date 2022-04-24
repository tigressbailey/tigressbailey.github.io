---
title: Replace moment.js and Happy New Year
date: "2021-12-18T12:40:32.169Z"
template: "post"
draft: false
slug: "replace-moment-js-and-happy-new-year"
category: "Frontend"
tags:
  - "Frontend"
  - "Web Development"
description: ""
# socialImage: "/media/42-line-bible.jpg"
socialImage: ""
---

Part one - Replace moment.js

I've been using moment.js since I got my first job.

This year, it has been replaced by date-fns in all of our projects.

The benefit is our bundle size has been reduced from moment+moment-timezone(Gzip 172kb) to less than date-fns+date-fns-tz(Gzip 22kb)

Why did I say it less than 22kb? Because date-fns supports tree-shaking naturally.

Here are techniques:

1. Technical selection
   
    [You-Dont-Need-Momentjs](https://github.com/you-dont-need/You-Dont-Need-Momentjs#brief-comparison)

    This inspires me and provides the courage to me.

    I struggled about choosing dayjs or date-fns.

    Both of them are great libraries. 
    
    According to [feature parity](https://github.com/you-dont-need/You-Dont-Need-Momentjs#feature-parity), date-fns has the same features comparing to moment.js, in which case, no workarounds and no 3rd parties are required for the replacement.

    So my selection is date-fns.

2. Replace moment formats with date-fns formats
   The formats indicate different outputs.

   Compare moment and date-fns and create alias between them would speed up the progress.

3. Replace utils
    [feature parity](https://github.com/you-dont-need/You-Dont-Need-Momentjs#feature-parity) covers 90% utils replacement.
   
4. Rebuild antd components
   Ant Design heavily use moment.js and moment-timezone library.

   It provides some solutions in reference to https://ant.design/docs/react/replace-moment.

   But it has a bug that when the customized datepicker component is created using the snippet.

   ```Typescript
    import dateFnsGenerateConfig from 'rc-picker/lib/generate/dateFns'
    import generatePicker from 'antd/es/date-picker/generatePicker'
    import 'antd/es/date-picker/style/index'

    const DatePicker = generatePicker<Date>(dateFnsGenerateConfig)

    export default DatePicker;
   ```

   It actually imports all locales from date-fns-tz in the rc-picker/lib/generate/dateFns file (https://github.com/react-component/picker/blob/dc21474ad9a7af81ba20f1445e4acdd6f746479b/src/generate/dateFns.ts#L26)
   ```Typescript
   import * as Locale from 'date-fns/locale'
   ```

   This is not as expected.
   I created our own generate/dateFns file instead.
   
   ```Typescript
    import {
      getDay,
      getYear,
      getMonth,
      getDate,
      endOfMonth,
      getHours,
      getMinutes,
      getSeconds,
      addYears,
      addMonths,
      addDays,
      setYear,
      setMonth,
      setDate,
      setHours,
      setMinutes,
      setSeconds,
      isAfter,
      isValid,
      getWeek,
      startOfWeek,
      format as formatDate,
      parse as parseDate,
    } from 'date-fns'
    import { enUS, ja, zhCN, zhTW } from 'date-fns/locale'
    import { GenerateConfig } from './generateConfig'

    /* eslint-disable camelcase */
    /* eslint-disable @typescript-eslint/explicit-module-boundary-types */
    const Locale = { enUS, ja, zhTW, zhCN }
    const DEFAULT_LOCALE = 'enUS'

    const LOCALES_MAP = {
      en: 'enUS',
      en_US: 'enUS',
      ja_JP: 'ja',
      zh: 'zhTW',
      zh_TW: 'zhTW',
      zh_CN: 'zhCN',
    }

    const dealLocal = (str: string): string => LOCALES_MAP[str] || DEFAULT_LOCALE

    const localeParse = (format: string) => {
      return format
        .replace(/Y/g, 'y')
        .replace(/D/g, 'd')
        .replace(/gggg/, 'yyyy')
        .replace(/g/g, 'G')
        .replace(/([Ww])o/g, 'wo')
    }

    const generateConfig: GenerateConfig<Date> = {
      // get
      getNow: () => new Date(),
      getFixedDate: string => new Date(string),
      getEndDate: date => endOfMonth(date),
      getWeekDay: date => getDay(date),
      getYear: date => getYear(date),
      getMonth: date => getMonth(date),
      getDate: date => getDate(date),
      getHour: date => getHours(date),
      getMinute: date => getMinutes(date),
      getSecond: date => getSeconds(date),

      // set
      addYear: (date, diff) => addYears(date, diff),
      addMonth: (date, diff) => addMonths(date, diff),
      addDate: (date, diff) => addDays(date, diff),
      setYear: (date, year) => setYear(date, year),
      setMonth: (date, month) => setMonth(date, month),
      setDate: (date, num) => setDate(date, num),
      setHour: (date, hour) => setHours(date, hour),
      setMinute: (date, minute) => setMinutes(date, minute),
      setSecond: (date, second) => setSeconds(date, second),

      // Compare
      isAfter: (date1, date2) => isAfter(date1, date2),
      isValidate: date => isValid(date),

      locale: {
        getWeekFirstDay: locale => {
          const clone = Locale[dealLocal(locale)]
          return clone.options.weekStartsOn
        },
        getWeekFirstDate: (locale, date) => {
          return startOfWeek(date, { locale: Locale[dealLocal(locale)] })
        },
        getWeek: (locale, date) => {
          return getWeek(date, { locale: Locale[dealLocal(locale)] })
        },
        getShortWeekDays: locale => {
          const clone = Locale[dealLocal(locale)]
          return Array.from({ length: 7 }).map((_, i) =>
            clone.localize.day(i, { width: 'short' })
          )
        },
        getShortMonths: locale => {
          const clone = Locale[dealLocal(locale)]
          return Array.from({ length: 12 }).map((_, i) =>
            clone.localize.month(i, { width: 'abbreviated' })
          )
        },
        format: (locale, date, format) => {
          if (!isValid(date)) {
            return null
          }
          return formatDate(date, localeParse(format), {
            locale: Locale[dealLocal(locale)],
          })
        },
        parse: (locale, text, formats) => {
          for (let i = 0; i < formats.length; i += 1) {
            const format = localeParse(formats[i])
            const formatText = text
            const date = parseDate(formatText, format, new Date(), {
              locale: Locale[dealLocal(locale)],
            })
            if (isValid(date)) {
              return date
            }
          }
          return null
        },
      },
    }

    export default generateConfig
   ```

   Then we could use our own config.

   ```Typescript
    import generatePicker from 'antd/lib/date-picker/generatePicker'
    import dateFnsGenerateConfig from '@/components/forms/DateFnsComponents/dateFnsGenerateConfig'
    import 'antd/lib/date-picker/style/index'

    const DatePicker = generatePicker<Date>(dateFnsGenerateConfig)

    export default DatePicker
   ```

5. Check if any other libraries reused Ant Design's DatePicker or Calendar component.
   For instance, we use a json schema form library and it uses them. I recreated the component and replace the moment.js as well.

---
Part two - Happy new year!

I hung out with my dearest friends last weekend.

We had a great time during Christmas.

All the songs, fireworks, laughs and joys we shared together.

I recalled the start of this year. Someone copy my work achievement and put it on his blog. I was being really upset about it when reading that post for the first time. Then I realized that post actually didn't write the key point although my code was copied 99%. The missing 1% would make the achievement failed entirely.
Why would the code be posted as it is not working? It won't help others.

My friend told me that I'm always the creator and yes I am.

This year I meet some great people, expertise Next.js and experience with GatsbyJs.
I really enjoy my work, coding and the collaborations.
Every time our BE fellows told me they are BFF(backend for frontend). I just want to say we are BFF(best friends forever).

I wish everyone a happy new year in advance.

I will continue with English writing and include Flutter into my skill list next year.


