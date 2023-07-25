# Intl ZonedDateTimeFormat Proposal

This is a proposal for Intl.ZonedDateTimeFormat, a new format to support the formating of Temporal.ZonedDateTime object.

# Stage
Stage 1

* [Slides for Stage 1 Advancement for TC39 2023 May Meeting](https://docs.google.com/presentation/d/1JhEFhB4wUkHfuNeJlCPUaxBv2oGJOKnaF2j8jfmhdi0)
  * [TG2 May 2023 Discussion](https://github.com/tc39/ecma402/blob/master/meetings/notes-2023-04-06.md#zoneddatetimeformat)
  * [TC39 May 2023 Discussion](https://github.com/tc39/notes/blob/main/meetings/2023-05/may-17.md#intlzoneddatetimeformat-for-stage-1)

# Moviation

[Temporal proposal](hhttps://tc39.es/proposal-temporal/) enhances the pre-existing Intl.DateTimeFormat to support some, but not all, Temporal objects:
* Temporal.PlainDateTime
* Temporal.PlainDate
* Temporal.PlainTime
* Temporal.PlainMonthDay
* Temporal.PlainYearMonth
* Temporal.Instant

Other Temporal objects are prosposed NOT to be be formatted by Intl.DateTimeFormat:
* Temporal.Calendar 
* Temporal.TimeZone
* Temporal.Duration: Formatted by [Intl.DurationFormat](https://tc39.es/proposal-intl-duration-format/)

There was an earlier attempt in the Temporal proposal to change Intl.DateTimeFormat to also format Temporal.ZonedDateTime but c[annot reach consensus after endless dicussion about how to resolve the timeZone from both objects](https://github.com/tc39/proposal-temporal/pull/2479#issuecomment-1472407831).

This proposal proposes a new Intl formatter, Intl.ZonedDateTimeFormat, designed to format [Temporal.ZonedDateTime](https://tc39.es/proposal-temporal/#sec-temporal-zoneddatetime-objects) objects and leave Intl.DateTimeFormat not to support the format of Temporal.ZonedDateTime.

# Alternative Consideration

## What Temporal.ZonedDateTime.prototype.toLocaleString cannot?

Two reasons why we cannot just use Temporal.ZonedDateTime.prototype.toLocaleString but need a new object:
* The need to format a string to represent the time period between TWO Temporal.ZonedDateTime. toLocaleString can only format string for one of them, but not the range between two Temporal.ZonedDateTime. The formatRange (and formtRangeToParts) function will be able to do so in this object.
* Some of the important information could be computed and cashed into the object to speed up some performance during the format function.

## Why not just Intl.DateTimeFormat?
Intl.DateTimeFormat was designed to format the Date object which does not contains a timeZone nor a Calendar. The TimeZone and Calendar of the Intl.DateTimeFormat therefore were resolved during the time of the object construction. Temporal.ZonedDateTime, in the other hand, always carry a TimeZone. Reusing Intl.DateTimeFormat to format Temporal.ZonedDateTime would cause conflict of timeZone and damage the purity of the Intl.DateTimeFormat API.

# API Proposal
## Intl.ZonedDateTimeFormat constructor
* Same as Intl.DateTimeFormat except it will throw RangeError if timeZone is presented in the option bag.
## Intl.ZonedDateTimeFormat.supportedLocales()
## Intl.ZonedDateTimeFormat.prototype.resolvedOptions()
* without return timeZone

## Intl.ZonedDateTimeFormat.prototype.format(x)
* Throw TypeError if x is not a Temporal.ZonedDateTime 
* (Later on, we may decide to accept a string which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatToParts(x)
* Throw TypeError if x is not a Temporal.ZonedDateTime 
* (Later on, we may decide to accept a string which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatRange(x, y)
* Throw TypeError if x is not a Temporal.ZonedDateTime 
* Throw TypeError if y is not a Temporal.ZonedDateTime 
* Throw RangeError if the timeZone of x and y are different
* (Later on, we may decide to accept string as x or y which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatRangeToParts(x, y)
* Throw TypeError if x is not a Temporal.ZonedDateTime 
* Throw TypeError if y is not a Temporal.ZonedDateTime 
* Throw RangeError if the timeZone of x and y are different
* (Later on, we may decide to accept string as x or y which can be used to construct a Temporal.ZonedDateTime )

## ~~Intl.ZonedDateTimeFormat.prototype.withTimeZone( [ timeZone ] )~~
* ~~timeZone could be a valid IANA timezone String or undefined (to indicate the current user timezone).~~
* ~~return an Intl.DateTimeFormat with the timeZone set to the string or undefined~~

## ~~Intl.DateTimeFormat.prototype.toZonedDateTimeFormat()~~
* ~~return an Intl.ZonedDateTimeFormat ignoring the timeZone in the internal slot.~~

<hr/>

# Below should be removed later
## Before creating a proposal

Please ensure the following:
  1. You have read the [process document](https://tc39.github.io/process-document/)
  1. You have reviewed the [existing proposals](https://github.com/tc39/proposals/)
  1. You are aware that your proposal requires being a member of TC39, or locating a TC39 delegate to "champion" your proposal

## Create your proposal repo

Follow these steps:
  1. Click the green ["use this template"](https://github.com/tc39/template-for-proposals/generate) button in the repo header. (Note: Do not fork this repo in GitHub's web interface, as that will later prevent transfer into the TC39 organization)
  1. Update the biblio to the latest version: `npm install --save-dev --save-exact @tc39/ecma262-biblio@latest`.
  1. Go to your repo settings “Options” page, under “GitHub Pages”, and set the source to the **main branch** under the root (and click Save, if it does not autosave this setting)
      1. check "Enforce HTTPS"
      1. On "Options", under "Features", Ensure "Issues" is checked, and disable "Wiki", and "Projects" (unless you intend to use Projects)
      1. Under "Merge button", check "automatically delete head branches"
<!--
  1. Avoid merge conflicts with build process output files by running:
      ```sh
      git config --local --add merge.output.driver true
      git config --local --add merge.output.driver true
      ```
  1. Add a post-rewrite git hook to auto-rebuild the output on every commit:
      ```sh
      cp hooks/post-rewrite .git/hooks/post-rewrite
      chmod +x .git/hooks/post-rewrite
      ```
-->
  3. ["How to write a good explainer"][explainer] explains how to make a good first impression.

      > Each TC39 proposal should have a `README.md` file which explains the purpose
      > of the proposal and its shape at a high level.
      >
      > ...
      >
      > The rest of this page can be used as a template ...

      Your explainer can point readers to the `index.html` generated from `spec.emu`
      via markdown like

      ```markdown
      You can browse the [ecmarkup output](https://ACCOUNT.github.io/PROJECT/)
      or browse the [source](https://github.com/ACCOUNT/PROJECT/blob/HEAD/spec.emu).
      ```

      where *ACCOUNT* and *PROJECT* are the first two path elements in your project's Github URL.
      For example, for github.com/**tc39**/**template-for-proposals**, *ACCOUNT* is "tc39"
      and *PROJECT* is "template-for-proposals".


## Maintain your proposal repo

  1. Make your changes to `spec.emu` (ecmarkup uses HTML syntax, but is not HTML, so I strongly suggest not naming it ".html")
  1. Any commit that makes meaningful changes to the spec, should run `npm run build` and commit the resulting output.
  1. Whenever you update `ecmarkup`, run `npm run build` and commit any changes that come from that dependency.

  [explainer]: https://github.com/tc39/how-we-work/blob/HEAD/explainer.md
