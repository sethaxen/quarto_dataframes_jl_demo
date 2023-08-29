# DataFrames rendering test

The following shows a table when rendered to PDF or HTML but nothing
when rendered to markdown

``` julia
using DataFrames
df = DataFrame(:x => randn(10), :y => randn(10))
```

But there is a custom `show` method for HTML outputs:

``` julia
sprint(show, "text/html", df)
```

    "<div><div style = \"float: left;\"><span>10×2 DataFrame</span></div><div style = \"clear: both;\"></div></div><div class = \"data-frame\" style = \"overflow-x: scroll;\"><table class = \"data-frame\" style = \"margin-bottom: 6px;\"><thead><tr class = \"header\"><th class = \"rowNumber" ⋯ 1948 bytes ⋯ "\">-0.209977</td><td style = \"text-align: right;\">0.846467</td></tr><tr><td class = \"rowNumber\" style = \"font-weight: bold; text-align: right;\">10</td><td style = \"text-align: right;\">0.85839</td><td style = \"text-align: right;\">0.00953941</td></tr></tbody></table></div>"

In fact, if we just wrap the `DataFrame` with an object that has a
custom HTML `show` method, it renders in markdown just fine:

``` julia
struct DataFrameWrapper
    df
end

Base.show(io::IO, mime::MIME"text/html", w::DataFrameWrapper) = show(io, mime, w.df)

DataFrameWrapper(df)
```

<div><div style = "float: left;"><span>10×2 DataFrame</span></div><div style = "clear: both;"></div></div><div class = "data-frame" style = "overflow-x: scroll;">

<table class="data-frame" data-quarto-postprocess="true"
style="margin-bottom: 6px;">
<thead>
<tr class="header header">
<th class="rowNumber" data-quarto-table-cell-role="th"
style="text-align: right; font-weight: bold;">Row</th>
<th style="text-align: left;" data-quarto-table-cell-role="th">x</th>
<th style="text-align: left;" data-quarto-table-cell-role="th">y</th>
</tr>
<tr class="odd subheader headerLastRow">
<th class="rowNumber" data-quarto-table-cell-role="th"
style="text-align: right; font-weight: bold;"></th>
<th style="text-align: left;" data-quarto-table-cell-role="th"
title="Float64">Float64</th>
<th style="text-align: left;" data-quarto-table-cell-role="th"
title="Float64">Float64</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">1</td>
<td style="text-align: right;">2.51882</td>
<td style="text-align: right;">1.17575</td>
</tr>
<tr class="even">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">2</td>
<td style="text-align: right;">-0.352146</td>
<td style="text-align: right;">0.0407075</td>
</tr>
<tr class="odd">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">3</td>
<td style="text-align: right;">-0.764117</td>
<td style="text-align: right;">0.359607</td>
</tr>
<tr class="even">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">4</td>
<td style="text-align: right;">-1.13374</td>
<td style="text-align: right;">0.0868884</td>
</tr>
<tr class="odd">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">5</td>
<td style="text-align: right;">-0.443483</td>
<td style="text-align: right;">-2.76214</td>
</tr>
<tr class="even">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">6</td>
<td style="text-align: right;">1.91845</td>
<td style="text-align: right;">-0.660005</td>
</tr>
<tr class="odd">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">7</td>
<td style="text-align: right;">0.917474</td>
<td style="text-align: right;">0.449895</td>
</tr>
<tr class="even">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">8</td>
<td style="text-align: right;">-0.288958</td>
<td style="text-align: right;">-0.525565</td>
</tr>
<tr class="odd">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">9</td>
<td style="text-align: right;">-0.209977</td>
<td style="text-align: right;">0.846467</td>
</tr>
<tr class="even">
<td class="rowNumber"
style="text-align: right; font-weight: bold;">10</td>
<td style="text-align: right;">0.85839</td>
<td style="text-align: right;">0.00953941</td>
</tr>
</tbody>
</table>

</div>
