---
layout: post
title: Project Highlights
---

For all codes related to this project, please visit the following git repository :


## Table of Contents
- [Data Acquisition and Visualisation](#1-data-acquisition-and-visualisation)
  - [Data Acquisition](#11-data-acquisition)
  - [Visualisations](#12-simple-visualizations)
- [Feature Engineering and Model Training](#2-feature-engineering-and-model-training)
  - [Shot Angle and Shot Distance plots](#21-histogram-of-shot-counts-binned-by-shot-distance-and-shot-angle)
- [Video Demo of running the application with the deplopyed Model](#3-application-demo-using-live-nhl-game)

## 1. Data Acquisition and Visualisation
**NOTE** : A more detailed version of this section is documented in the this [link]({{ site.url }}{{ site.baseurl }}/2024/09/17/milestone-1).

### 1.1 Data Acquisition
In this section, we first download the play-by-play events for each game using the publicly available NHL API:

[https://api-web.nhle.com/v1/gamecenter/{GAME_ID}/play-by-play](https://api-web.nhle.com/v1/gamecenter/{GAME_ID}/play-by-play)

To fetch the play-by-play events of a game of your choice, it is recommended to follow the following format:

Figuring out Game IDs for each season:

Since we download play-by-play events from each game (Regular and Playoffs) from seasons 2016-2017 to 2023-2024, we break-down the logic behind the naming of GAME_ID as mentioned in the following link :

[NHL API Game IDs Documentation](https://gitlab.com/dword4/nhlapi/-/blob/master/stats-api.md#game-ids)

In brief, suppose we take GAME_ID ``` 2019020901 ``` and GAME_ID ``` 2021030217 ``` , the breakdown from left to right would be as follows:

``` 2019020901 ```
- '2019' for the season 2019-2020.
- '02' for regular season.
- '0901' for game number 901 in the regular season.

``` 2021030217 ```
- '2021' for the season 2021-2022.
- '03' for the playoff season.
- '0217' -> For playoff games, the 2nd digit of the specific number gives the round of the playoffs, the 3rd digit specifies the matchup, and the 4th digit specifies the game (out of 7). (In this example: 7th game of match #1 in playoff round 2.)

In our project, we ping each game and cache the play-by-play events for ALL NHL games from seasons 2016-2017 to 2023-2024 (including regular and playoffs games!). For more explanations, we have a [detailed web-page]({{ site.url }}{{ site.baseurl }}/2024/09/17/milestone-1) dedicated to this section.

### 1.2 Simple Visualizations

All the following plots were obtained using the cumulative play-by-play data from all seasons and games mentioned in the previous section.
#### Shot types

**NOTE** : For a much more detailed analysis of all simple visualisations, follow this [link]({{ site.baseurl }}/2024/09/17/milestone-1/#shot-types).
Important note: for this analysis, we have decided to drop shot types that have been used less than 0.1%, because they don't represent meaningful information, especially when compared to other shot types.
The shot types dropped were "between-legs" and "cradle", with 0.06% and 0.005% usage, respectively.

<img src="{{ site.baseurl }}/assets/images/simple_viz_shot_types.png" alt="2023-2044 Season Shot Types Bar plot">

#### Goal rate vs distance 

**NOTE** for detailed explanations for this section follow this [link]({{ site.baseurl }}/2024/09/17/milestone-1/#goal-distance)

<img src="{{ site.baseurl }}/assets/images/simple_viz_goal_conversion.png" alt="Goal Conversion Rate for Seasons 2018, 2019 and 2020">

#### Shot vs distance and shot-type

**NOTE** for detailed explanations for this section follow this [link]({{ site.baseurl }}/2024/09/17/milestone-1/#shot-distance)

<img src="{{ site.baseurl }}/assets/images/simple_viz_goal_conversion_vs_dist1.png" alt="Goal Conversion Rate for Season 2023, per shot type">
<img src="{{ site.baseurl }}/assets/images/simple_viz_goal_conversion_vs_dist2.png" alt="Goal Conversion Rate for Season 2023, per shot type">

### 1.3 Advanced Visualisations

**NOTE** : For a more detailed interpretation of these plots, use this [link]({{ site.baseurl }}/2024/09/17/milestone-1/#advanced-viz)

For the advanced visualisations, we have decided to include missed shots in our calculations to get a more complete picture of offensive performance.

<!-- Dropdown Menu for Selecting Year -->
<div style="display: flex; align-items: center;">
  <h4 style="margin-right: 10px;">Select the season to display</h4>
  <select id="yearDropdown">
    <option value="2016">2016-2017</option>
    <option value="2017">2017-2018</option>
    <option value="2018">2018-2019</option>
    <option value="2019">2019-2020</option>
    <option value="2020">2020-2021</option>
  </select>
</div>

<!-- Hide the figures -->
<div id="plotly_2016" class="plotly-figure" style="display:none;">
  {% include plotly_2016.html %}
</div>
<div id="plotly_2017" class="plotly-figure" style="display:none;">
  {% include plotly_2017.html %}
</div>
<div id="plotly_2018" class="plotly-figure" style="display:none;">
  {% include plotly_2018.html %}
</div>
<div id="plotly_2019" class="plotly-figure" style="display:none;">
  {% include plotly_2019.html %}
</div>
<div id="plotly_2020" class="plotly-figure" style="display:none;">
  {% include plotly_2020.html %}
</div>

<!-- JavaScript for Toggling Figures -->
<script>
  document.getElementById('yearDropdown').addEventListener('change', function() {
    // get the selected year from the dropdown
    var selectedYear = this.value;

    // hide all figures
    var figures = document.querySelectorAll('.plotly-figure');
    figures.forEach(function(figure) {
      figure.style.display = 'none';
    });

    // show the figure that matches the selected year
    document.getElementById('plotly_' + selectedYear).style.display = 'block';
  });

  // show the first figure by default
  document.getElementById('plotly_2016').style.display = 'block';
</script>

## 2. Feature Engineering and Model Training

### 2.1 Histogram of shot counts binned by shot distance and shot angle

**NOTE**: The 'Non-Goals' are shots classified as 'shot-on-goal' and 'missed-shots' in our data for seasons from 2016-2017 upto 2019-2020(4 seasons). Additionally, shot angles are higher for shots that are taken head-on. For example, a shot angle of 90 degrees means it is directed along the same line as taking a shot from the centre of the pitch towards goal.

Plotting histogram of shot counts binned by shot distance:

<img src="{{ site.baseurl }}/assets/images/Feature1_binshots_dist.png" alt="Goals by distance">
<img src="{{ site.baseurl }}/assets/images/Feature1_binshotNg_distance.png" alt="Non-goals by distance">

A more detailed version of this section is documented in the this [link]({{ site.url }}{{ site.baseurl }}/2024/11/13/milestone-2)

Plotting histogram of shot counts binned by shot angle:

<img src="{{ site.baseurl }}/assets/images/Feature1_binshotG_angle.png" alt="Goals by angle">
<img src="{{ site.baseurl }}/assets/images/Feature1_binshotsNg_angle.png" alt="Non-goals by angle">

### 2.2 Plotting Goal Rate (Goal/(No Goals + Goals)) binned by distance and shot angle

Goal rate by shot distance:

<img src="{{ site.baseurl }}/assets/images/Feature1_bingoals_distance.png" alt="Goal rate by distance">

Goal rate by shot angle:

<img src="{{ site.baseurl }}/assets/images/Feature1_bingoals_angle.png" alt="Goal rate by angle">

### 2.3 Plotting Histograms of empty and non-empty goals

<img src="{{ site.baseurl }}/assets/images/Feature1_emptygoals_dist.png" alt="Empty goals by distance">

<img src="{{ site.baseurl }}/assets/images/Feature1_nonempty_distance.png" alt="Non Empty goals by distance">

## 3 Application Demo using live NHL game
The video starts by starting both containers with the `docker-compose up` command.  
Then, the client app is demonstrated into the browser. The user can select a model and the game ID. The game ID selected was actually live during the recording of the video ([January 13th, 2025, Calgary Flames vs. Chicago Blackhawks](https://www.nhl.com/gamecenter/cgy-vs-chi/2025/01/13/2024020690/boxscore)).  
We can see that the first select model predicts "0.4" goals for the Flames, and "0.6" goals for the Blackhawks, while the score was 1-1 when recording the video (there was 13 minutes left in the first period).  

<iframe width="420" height="315" src="https://youtube.com/embed/GKGzfGlOhJI" frameborder="0" allowfullscreen></iframe>
