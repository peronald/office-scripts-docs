---
title: 'Troubleshooting information for Power Automate with Office Scripts'
description: 'Tips, platform information, and known issues with the integration between Office Scripts and Power Automate.'
ms.date: 01/14/2021
localization_priority: Normal
---

# Troubleshooting information for Power Automate with Office Scripts

Power Automate lets you take your Office Script automation to the next level. However, because Power Automate runs scripts on your behalf in independent Excel sessions, there are a few important things to note.

> [!TIP]
> If you're just starting to use Office Scripts with Power Automate, please start with [Run Office Scripts with Power Automate](../develop/power-automate-integration.md) to learn about the platforms.

## Avoid using relative references

Power Automate runs your script in the chosen Excel workbook on your behalf. The workbook might be closed when this happens. Any API that relies on the user's current state, such as `Workbook.getActiveWorksheet`, may behave differently in Power Automate. This is because the APIs are based on a relative position of the user's view or cursor and that reference doesn't exist in a Power Automate flow.

Some relative reference APIs throw errors in Power Automate. Others have a default behavior that implies a user's state. When designing your scripts, be sure to use absolute references for worksheets and ranges. This makes your Power Automate flow consistent, even if worksheets are rearranged.

### Script methods that fail when run Power Automate flows

The following methods will throw an error and fail when called from a script in a Power Automate flow.

| Class | Method |
|--|--|
| [Chart](/javascript/api/office-scripts/excelscript/excelscript.chart) | `activate` |
| [Range](/javascript/api/office-scripts/excelscript/excelscript.range) | `select` |
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getActiveCell` |
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getActiveChart` |
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getActiveSlicer` |
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getSelectedRange` |
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getSelectedRanges` |

### Script methods with a default behavior in Power Automate flows

The following methods use a default behavior, in lieu of any user's current state.

| Class | Method | Power Automate behavior |
|--|--|--|
| [Workbook](/javascript/api/office-scripts/excelscript/excelscript.workbook) | `getActiveWorksheet` | Returns either the first worksheet in the workbook or the worksheet currently activated by the `Worksheet.activate` method. |
| [Worksheet](/javascript/api/office-scripts/excelscript/excelscript.worksheet) | `activate` | Marks the worksheet as the active worksheet for purposes of `Workbook.getActiveWorksheet`. |

## Select workbooks with the file browser control

When building the **Run script** step of a Power Automate flow, you need to select which workbook is part of the flow. Use the file browser to select your workbook, instead of manually typing the workbook's name.

:::image type="content" source="../images/power-automate-file-browser.png" alt-text="The Power Automate Run script action showing the Show Picker file browser option":::

For more context on the Power Automate limitation and a discussion of potential workarounds for the dynamic selection of workbooks, see [this thread in the Microsoft Power Automate Community](https://powerusers.microsoft.com/t5/Power-Automate-Ideas/Allow-for-dynamic-quot-file-quot-value-for-excel-quot-get-a-row/idi-p/103091#).

## Time zone differences

Excel files don't have an inherent location or timezone. Every time a user opens the workbook, their session uses that user's local timezone for date calculations. Power Automate always uses UTC.

If your script uses dates or times, there may be behavioral differences when the script is tested locally versus when it is run through Power Automate. Power Automate allows you to convert, format, and adjust times. See [Working with Dates and Times inside of your flows](https://flow.microsoft.com/blog/working-with-dates-and-times/) for instructions on how to use those functions in Power Automate and [`main` Parameters: Passing data to a script](../develop/power-automate-integration.md#main-parameters-passing-data-to-a-script) to learn how to provide that time information for the script.

## See also

- [Troubleshooting Office Scripts](troubleshooting.md)
- [Run Office Scripts with Power Automate](../develop/power-automate-integration.md)
- [Excel Online (Business) connector reference documentation](/connectors/excelonlinebusiness/)
