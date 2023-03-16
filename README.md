# Intl ZonedDateTimeFormat Proposal

This is a proposal for Intl.ZonedDateTimeFormat, a new format to support the formating of Temporal.ZonedDateTime object.

# Stage
Stage 0

# Moviation

[Temporal proposal](hhttps://tc39.es/proposal-temporal/) to enhance the pre-existing Intl.DateTimeFormat to support some, but not all, of the Temporal objects:
* Temporal.PlainDateTime
* Temporal.PlainDate
* Temporal.PlainTime
* Temporal.PlainMonthDay
* Temporal.PlainYearMonth
* Temporal.Instant

Other Temporal objects are not prosposed to be formatted by Intl.DateTimeFormat:
* Temporal.Calendar 
* Temporal.TimeZone
* Temporal.Duration: Formatted by [Intl.DurationFormat](https://tc39.es/proposal-intl-duration-format/)

There was an earlier attempt in the Temporal proposal to change Intl.DateTimeFormat to format Temporal.ZonedDateTime but cannot reach consensus after endless dicussion.

This proposal propose to use a new format Intl.ZonedDateTimeFormat to format [Temporal.ZonedDateTime](https://tc39.es/proposal-temporal/#sec-temporal-zoneddatetime-objects) objects and leave Intl.DateTimeFormat not to support the format of Temporal.ZonedDateTime.


# API Proposal
## Intl.ZonedDateTimeFormat constructor
Same as Intl.DateTimeFormat except it will throw RangeError if timeZone is presented in the option bag.
## Intl.ZonedDateTimeFormat.supportedLocales()
## Intl.ZonedDateTimeFormat.prototype.resolvedOptions()
without return timeZone

## Intl.ZonedDateTimeFormat.prototype.format(x)
Throw TypeError if x is not a Temporal.ZonedDateTime 
(Later on, we may decide to accept a string which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatToParts(x)
Throw TypeError if x is not a Temporal.ZonedDateTime 
(Later on, we may decide to accept a string which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatRange(x, y)
Throw TypeError if x is not a Temporal.ZonedDateTime 
Throw TypeError if y is not a Temporal.ZonedDateTime 
Throw RangeError if the timeZone of x and y are different
(Later on, we may decide to accept string as x or y which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.formatRangeToParts(x, y)
Throw TypeError if x is not a Temporal.ZonedDateTime 
Throw TypeError if y is not a Temporal.ZonedDateTime 
Throw RangeError if the timeZone of x and y are different
(Later on, we may decide to accept string as x or y which can be used to construct a Temporal.ZonedDateTime )

## Intl.ZonedDateTimeFormat.prototype.withTimeZone( [ timeZone ] )
timeZone could be a valid IANA timezone String, a Temporal.TimeZone or undefined (to indicate the current user timezone).
return an Intl.DateTimeFormat with the timeZone set to the string or undefined

## Intl.DateTimeFormat.prototype.toZonedDateTimeFormat()
return an Intl.ZonedDateTimeFormat ignoring the timeZone in the internal slot.



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
