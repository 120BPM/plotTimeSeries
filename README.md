# plot_time_series

The plot_time_series is a simple utility for plotting a time_series graph using R
with some flexibility. This is handy for e.g. plotting KPI values over time.

## Installation

To use this script, you need to [install R](http://cran.r-project.org/mirrors.html) and then get the `getopt` package. 
To install the `getopt` package, fire up R (type R in your terminal) and type in something like:

    install.packages('getopt', repos='http://cran.us.r-project.org')

If your R binary is somewhere other than `/usr/bin` you'll need to edit the first line of `plot_time_series` to point it at the correct location. Do `which R` to find it.

You can use plot_time_series anywhere you can use R, but the tests are *nix flavored.

## Usage

    NAME
          plot_time_series
    
    SYNOPSIS
           Usage: ../plot_time_series [-[-verbose|v]] [-[-help|h]] [-[-show_gridlines|g]] [-[-remove_outliers|o]] [-[-trim_to_week|T]] [-[-csv_filename|c] <character>] [-[-special_points_filename|S] [<character>]] [-[-special_points_color|C] [<character>]] [-[-output_filename|f] <character>] [-[-input_date_format|d] <character>] [-[-title|t] <character>] [-[-y_title|y] <character>] [-[-y_unit|s] <character>] [-[-y_range|Y] [<character>]] [-[-x_range|X] [<character>]] [-[-x_spacing|x] [<character>]] [-[-smoothness|m] [<double>]] [-[-width|w] [<integer>]] [-[-height|u] [<integer>]] [-[-point_color|p] [<character>]] [-[-sunday_point_color|z] [<character>]] [-[-axis_color|a] [<character>]]
    
    DESCRIPTION
          The plot_time_series is a simple utility for plotting a time series graph using R.
          This is useful for e.g. plotting KPI values over time.
          The graph is drawn as a scatterplot with a LOESS fit line of variable smoothness.
    
    OPTIONS
          The options are as follows:
    
          --verbose                    Get a little chatty?
          --width                      The width of the graph in pixels
          --height                     The height of the graph in pixels
          --csv_filename               A CSV containing input data with columns date,value (example below)
          --output_filename            The file that the graph is written to
          --special_points_filename    A file containing special points e.g. milestones or targets
          --special_points_color       The color of the special points
          --title                      The title shown above the graph
          --y_title                    The title shown on the y axis of the graph
          --y_unit                     A string appended to the y axis values e.g. MM
          --y_range                    Manually set Y axis limits e.g. 10:1000
          --x_range                    Manually set X axis limits e.g. 04/27/2012:05/20/2013
          --x_spacing                  How the x-axis is spaced e.g. day, 3 days, week, month, 2 weeks, 2 months
          --point_color                The color to draw points in
          --sunday_point_color         The color to draw sunday points in
          --input_date_format          The format of the date in the datafile, default is "%m/%d/%Y"
          --axis_color                 The color to use for the axis
          --smoothness                 How much to smooth the LOESS fit line. Float between 0 and 1
          --remove_outliers            Automagically remove outlying points?
          --trim_to_week               Trim the X axis to whole weeks? (x grid goes Sunday to Sunday)
          --show_gridlines             Show gridlines?
    
    EXAMPLES
          $ plot_time_series --width=800 --height=400 --x_spacing=month --csv_filename=test_data/unbranded_organic_registrations.csv --output_filename=output/unbranded_organic_registrations.png --title="Unbranded Organic Registrations: %s to %s" --y_title="Number Registrations"
    
    
    EXAMPLE INPUT FILE
          date,value
          03/06/2012,6347.04
          03/07/2012,9990.23
          03/08/2012,6773.41
    
    COLOR
          Available colors documented at: http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf
    

## Testing

There is a test directory with some lovely testy stuff in it:

    test/test_data - a directory with some test csv files in it
    test/output    - a directory that the test script dumps images into
    run_test       - a bash script that runs the tests
    tests.txt      - the test definitions

Go and run the tests and make sure everything passes:

    $ cd test
    $ ./run_test 
      * running test "registrations thumb" ...  passed
      * running test "registrations graph" ...  passed
      * running test "revenue thumb" ...  passed
      * running test "revenue graph" ...  passed
      * running test "revenue graph with targets" ...  passed
      * running test "registrations thumb" ...  passed
    
      ** All tests passed
## Examples

Example graphs are shown below. These are produced by from the data in the `test` directory

#### A simple graph showing registrations by time
!["registrations thumb"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_registrations_thumb.png)

     ../plot_time_series --width=500 --height=400 --x_spacing=month 
     --csv_filename=test_data/acme_registrations.csv --point_color=gray80 --sunday_point_color=gray80 
     --remove_outliers --smoothness=0.8

#### A detailed graph showing registrations by time
!["registrations graph"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_registrations.png)

     ../plot_time_series --width=1200 --height=700 --x_spacing=week 
     --csv_filename=test_data/acme_registrations.csv --show_gridlines --title="Acme Registrations: %s to 
     %s" --y_title="Number Registrations" --remove_outliers

#### A simple graph showing revenue by time
!["revenue thumb"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_revenue_thumb.png)

     ../plot_time_series --width=500 --height=400 --x_spacing="2 months" 
     --csv_filename=test_data/acme_revenue.csv --point_color=gray80 --sunday_point_color=gray80 
     --remove_outliers --smoothness=0.8

#### A detailed graph showing revenue by time
!["revenue graph"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_revenue.png)

     ../plot_time_series --width=1200 --height=700 --x_spacing=week 
     --csv_filename=test_data/acme_revenue.csv --show_gridlines --title="Acme Revenue: %s to %s" 
     --remove_outliers

#### A detailed graph showing revenue by time with targets
!["revenue graph with targets"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_revenue_targets.png)

     ../plot_time_series --width=1200 --height=700 --x_spacing=week 
     --csv_filename=test_data/acme_revenue.csv --show_gridlines --title="Acme Revenue: %s to %s" 
     --remove_outliers --special_points_filename=test_data/acme_revenue_targets.csv 
     --special_points_color=cornflowerblue --point_color=gray80 --sunday_point_color=gray80

#### A simple graph showing registrations by time from April to September
!["registrations thumb"](https://raw.github.com/doofdoofsf/plotTimeSeries/master/test/output/acme_registrations_thumb_dates.png)

     ../plot_time_series --width=500 --height=400 --x_spacing=month 
     --csv_filename=test_data/acme_registrations.csv --title="April to September" --point_color=gray80 
     --sunday_point_color=gray80 --remove_outliers --smoothness=0.2 --x_range=04/01/2012:09/01/2012 
     --y_range=50:250
