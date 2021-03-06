Student Forecast Simulation
================

This post attempts to provide an example using simulated data of how to
forecast student enrollment at a four-year college or university. It
uses Markov chains with simulated data. The complete code to reproduce
this post can be found
[here](https://github.com/kieran11/studentforecast/SimulatedForecast.Rmd)

## Describe a model:

In most North Americana four-year universities, students progress
through the four-year curriculum, and then graduate after four years.
However, some students leave after the first year, other after their
third year. Using a discrete markov chain model can be an easy tool to
forecast the number of students a given University will have the
following semester, or following year.

The `markovchain` package in `R` was developed and works well for
forecasting students in this scenario. The complete vigenette is
[here](https://cran.r-project.org/web/packages/markovchain/vignettes/an_introduction_to_markovchain_package.pdf).
The vignette provides the theoretical background, and practical
examples.

## Data

Below is a table showing an example of the student-level data. There is
a student identifier, their current semester, and their following
semester. The current semester and the following semester are the key
variables that will make up the transition matrix.

<!--html_preserve-->

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#wcqnjoskij .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#wcqnjoskij .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#wcqnjoskij .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#wcqnjoskij .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#wcqnjoskij .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#wcqnjoskij .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#wcqnjoskij .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#wcqnjoskij .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#wcqnjoskij .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#wcqnjoskij .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#wcqnjoskij .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#wcqnjoskij .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#wcqnjoskij .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#wcqnjoskij .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#wcqnjoskij .gt_from_md > :first-child {
  margin-top: 0;
}

#wcqnjoskij .gt_from_md > :last-child {
  margin-bottom: 0;
}

#wcqnjoskij .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#wcqnjoskij .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#wcqnjoskij .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#wcqnjoskij .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#wcqnjoskij .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#wcqnjoskij .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#wcqnjoskij .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#wcqnjoskij .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#wcqnjoskij .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#wcqnjoskij .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#wcqnjoskij .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#wcqnjoskij .gt_left {
  text-align: left;
}

#wcqnjoskij .gt_center {
  text-align: center;
}

#wcqnjoskij .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#wcqnjoskij .gt_font_normal {
  font-weight: normal;
}

#wcqnjoskij .gt_font_bold {
  font-weight: bold;
}

#wcqnjoskij .gt_font_italic {
  font-style: italic;
}

#wcqnjoskij .gt_super {
  font-size: 65%;
}

#wcqnjoskij .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>

<div id="wcqnjoskij" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">

<table class="gt_table">

<thead class="gt_header">

<tr>

<th colspan="7" class="gt_heading gt_title gt_font_normal" style>

Student-level data example

</th>

</tr>

<tr>

<th colspan="7" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style>

</th>

</tr>

</thead>

<thead class="gt_col_headings">

<tr>

<th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">

Id

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">

Semester

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

NumberSemester

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">

ProbOfMovingOn

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">

MovedOn

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">

CurrentSemester

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">

FutureSemester

</th>

</tr>

</thead>

<tbody class="gt_table_body">

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F

</td>

<td class="gt_row gt_center">

1

</td>

<td class="gt_row gt_right">

0.114

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F1

</td>

<td class="gt_row gt_left">

W1

</td>

</tr>

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W

</td>

<td class="gt_row gt_center">

1

</td>

<td class="gt_row gt_right">

0.622

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W1

</td>

<td class="gt_row gt_left">

F2

</td>

</tr>

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F

</td>

<td class="gt_row gt_center">

2

</td>

<td class="gt_row gt_right">

0.609

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F2

</td>

<td class="gt_row gt_left">

W2

</td>

</tr>

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W

</td>

<td class="gt_row gt_center">

2

</td>

<td class="gt_row gt_right">

0.623

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W2

</td>

<td class="gt_row gt_left">

F3

</td>

</tr>

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F

</td>

<td class="gt_row gt_center">

3

</td>

<td class="gt_row gt_right">

0.861

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

F3

</td>

<td class="gt_row gt_left">

W3

</td>

</tr>

<tr>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W

</td>

<td class="gt_row gt_center">

3

</td>

<td class="gt_row gt_right">

0.640

</td>

<td class="gt_row gt_right">

1

</td>

<td class="gt_row gt_left">

W3

</td>

<td class="gt_row gt_left">

Leave

</td>

</tr>

</tbody>

</table>

</div>

<!--/html_preserve-->

To create the transition matrix, we need to calculate the chances
students have from moving from one state to another. For instance, if a
student enters in their first semester, Fall 1 (F1), then we need to
calculate the probability of them moving to other states. In our
example, the following states could be Winter 1 (W1), or they could
leave the institution.

<!--html_preserve-->

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#cazappngls .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#cazappngls .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#cazappngls .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#cazappngls .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#cazappngls .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cazappngls .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#cazappngls .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#cazappngls .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#cazappngls .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#cazappngls .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#cazappngls .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#cazappngls .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#cazappngls .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#cazappngls .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#cazappngls .gt_from_md > :first-child {
  margin-top: 0;
}

#cazappngls .gt_from_md > :last-child {
  margin-bottom: 0;
}

#cazappngls .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#cazappngls .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#cazappngls .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cazappngls .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#cazappngls .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cazappngls .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#cazappngls .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cazappngls .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#cazappngls .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#cazappngls .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#cazappngls .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#cazappngls .gt_left {
  text-align: left;
}

#cazappngls .gt_center {
  text-align: center;
}

#cazappngls .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#cazappngls .gt_font_normal {
  font-weight: normal;
}

#cazappngls .gt_font_bold {
  font-weight: bold;
}

#cazappngls .gt_font_italic {
  font-style: italic;
}

#cazappngls .gt_super {
  font-size: 65%;
}

#cazappngls .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>

<div id="cazappngls" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">

<table class="gt_table">

<thead class="gt_header">

<tr>

<th colspan="11" class="gt_heading gt_title gt_font_normal" style>

Transition Matrix

</th>

</tr>

<tr>

<th colspan="11" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style>

</th>

</tr>

</thead>

<thead class="gt_col_headings">

<tr>

<th class="gt_col_heading gt_center gt_columns_bottom_border" rowspan="2" colspan="1">

CurrentSemester

</th>

<th class="gt_center gt_columns_top_border gt_column_spanner_outer" rowspan="1" colspan="10">

<span class="gt_column_spanner">Following Semester</span>

</th>

</tr>

<tr>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

F2

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

F3

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

F4

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

Graduated

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

Leave

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

Not graduated

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

W1

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

W2

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

W3

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">

W4

</th>

</tr>

</thead>

<tbody class="gt_table_body">

<tr>

<td class="gt_row gt_left">

F1

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.098

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.902

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F2

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.053

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.947

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F3

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.172

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.828

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F4

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.099

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.901

</td>

</tr>

<tr>

<td class="gt_row gt_left">

Leave

</td>

<td class="gt_row gt_right">

0.127

</td>

<td class="gt_row gt_right">

0.042

</td>

<td class="gt_row gt_right">

0.197

</td>

<td class="gt_row gt_right">

0.099

</td>

<td class="gt_row gt_right">

0.085

</td>

<td class="gt_row gt_right">

0.056

</td>

<td class="gt_row gt_right">

0.113

</td>

<td class="gt_row gt_right">

0.085

</td>

<td class="gt_row gt_right">

0.099

</td>

<td class="gt_row gt_right">

0.099

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W1

</td>

<td class="gt_row gt_right">

0.934

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.066

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W2

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.947

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.053

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W3

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.917

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.083

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W4

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.820

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.180

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

<td class="gt_row gt_right">

0.000

</td>

</tr>

</tbody>

</table>

</div>

<!--/html_preserve-->

## Making a forecast

To make a forecast, we need to input a number of students at a
particular state. For our example, we enter 100 students at F1. Given
the transition matrix, the number of students will continue to decline
until the process is complete.

The below table shows the final outcome.

As we can see, while we start with one hundred students, that decreases
over time.

<!--html_preserve-->

<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#fshsvnsejz .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#fshsvnsejz .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#fshsvnsejz .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#fshsvnsejz .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#fshsvnsejz .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#fshsvnsejz .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#fshsvnsejz .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#fshsvnsejz .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#fshsvnsejz .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#fshsvnsejz .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#fshsvnsejz .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#fshsvnsejz .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#fshsvnsejz .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#fshsvnsejz .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#fshsvnsejz .gt_from_md > :first-child {
  margin-top: 0;
}

#fshsvnsejz .gt_from_md > :last-child {
  margin-bottom: 0;
}

#fshsvnsejz .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#fshsvnsejz .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#fshsvnsejz .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#fshsvnsejz .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#fshsvnsejz .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#fshsvnsejz .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#fshsvnsejz .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#fshsvnsejz .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#fshsvnsejz .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#fshsvnsejz .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#fshsvnsejz .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#fshsvnsejz .gt_left {
  text-align: left;
}

#fshsvnsejz .gt_center {
  text-align: center;
}

#fshsvnsejz .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#fshsvnsejz .gt_font_normal {
  font-weight: normal;
}

#fshsvnsejz .gt_font_bold {
  font-weight: bold;
}

#fshsvnsejz .gt_font_italic {
  font-style: italic;
}

#fshsvnsejz .gt_super {
  font-size: 65%;
}

#fshsvnsejz .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>

<div id="fshsvnsejz" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">

<table class="gt_table">

<thead class="gt_header">

<tr>

<th colspan="3" class="gt_heading gt_title gt_font_normal" style>

Final Forecast

</th>

</tr>

<tr>

<th colspan="3" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style>

</th>

</tr>

</thead>

<thead class="gt_col_headings">

<tr>

<th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">

Semester

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">

Student Count

</th>

<th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">

SemesterName

</th>

</tr>

</thead>

<tbody class="gt_table_body">

<tr>

<td class="gt_row gt_left">

F1

</td>

<td class="gt_row gt_right">

100.00000

</td>

<td class="gt_row gt_left">

Fall-1

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F2

</td>

<td class="gt_row gt_right">

98.22689

</td>

<td class="gt_row gt_left">

Fall-2

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F3

</td>

<td class="gt_row gt_right">

94.05890

</td>

<td class="gt_row gt_left">

Fall-3

</td>

</tr>

<tr>

<td class="gt_row gt_left">

F4

</td>

<td class="gt_row gt_right">

85.15074

</td>

<td class="gt_row gt_left">

Fall-4

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W1

</td>

<td class="gt_row gt_right">

97.08060

</td>

<td class="gt_row gt_left">

Winter-1

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W2

</td>

<td class="gt_row gt_right">

97.74008

</td>

<td class="gt_row gt_left">

Winter-2

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W3

</td>

<td class="gt_row gt_right">

82.36622

</td>

<td class="gt_row gt_left">

Winter-3

</td>

</tr>

<tr>

<td class="gt_row gt_left">

W4

</td>

<td class="gt_row gt_right">

80.17097

</td>

<td class="gt_row gt_left">

Winter-4

</td>

</tr>

</tbody>

</table>

</div>

<!--/html_preserve-->

## Bootstrapping

![](SimulatedForecast_files/figure-gfm/PlotOfBoots-1.png)<!-- -->
