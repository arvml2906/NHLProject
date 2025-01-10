---
layout: post
title: Project Highlights
---

For all codes related to this project, please visit the following git repository :



## 1. Data Acquisition and Visualisation
**NOTE** : A more detailed version of this section is documented in the this [link]({{ site.url }}{{ site.baseurl }}/2024/09/17/milestone-1).

### 1.1 Data Acquisition
In this section, we first download the play-by-play events for each game using the publicly available NHL API:

[https://api-web.nhle.com/v1/gamecenter/{GAME_ID}/play-by-play](https://api-web.nhle.com/v1/gamecenter/{GAME_ID}/play-by-play)

To fetch the play-by-play events of a game of your choice, it is recommended to follow the following format:

- Figuring out Game IDs for each season:

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

