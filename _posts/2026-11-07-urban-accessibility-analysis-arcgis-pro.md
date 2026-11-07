---
layout: post
title: "GIS-Based Urban Accessibility Analysis Using ArcGIS Pro"
description: "Urban accessibility analysis using GIS should follow the street network. Compare straight-line distance with service areas, routing, and location-allocation."
card_text: "A clinic can sit a few hundred meters away on the map and still be a long walk if a river, railway, or motorway blocks the direct path. This article explains network-based accessibility and the ArcGIS Pro methods used to measure it."
card_image: /assets/images/accessibility-network-comparison.svg
card_alt: "Diagram comparing a straight line across a river with a longer path that follows streets to a bridge."
image:
  path: /assets/images/og-urban-accessibility.png
  width: 1200
  height: 630
  alt: "Urban accessibility analysis using GIS. Urban GIS Lab tutorial."
permalink: /urban-accessibility-analysis-arcgis-pro/
date: 2026-11-07 08:00:00 +0330
---

A service that looks close on a map may be difficult to reach. Rivers, railways, motorways, one-way streets, and missing pedestrian links sit between a household and a clinic, school, or station. Urban accessibility analysis using GIS measures that separation in a way that respects how people move.

This article compares straight-line distance with network-based access, then reviews Euclidean distance, network analysis, service areas, nearest facilities, origin-destination matrices, and location-allocation. It explains the logic. It is not a full software tutorial. The applied work is in <a href="{{ site.course.url }}" target="_blank" rel="noopener noreferrer">Urban Spatial Analysis &amp; Modeling with ArcGIS Pro</a>, which builds networks, routes, service areas, and facility-location models in ArcGIS Pro.

## In this article
{: .no_toc}

* contents
{:toc}

## What Is Urban Accessibility?

Urban accessibility is how readily people can reach the opportunities they need: a hospital, a school, a park, a workplace, or a transit stop. "Readily" has to be defined. It may mean distance, travel time, cost, or a trip that is possible on foot or by another mode.

The relationship has three parts: origins, such as homes or neighborhood centroids; destinations, such as facilities; and the cost of movement between them. A count of facilities in a district is not an accessibility measure. The facilities may sit beyond a barrier, or the people who need them may live in one corner of the zone. GIS accessibility analysis connects demand, supply, and the network.

## Why Accessibility Matters in Urban Planning

Plans for healthcare, schools, emergency response, and public transport are questions of reach as well as capacity. Two neighborhoods can have the same population and very different access. <a href="{% link _posts/2026-11-07-population-density-analysis-arcgis-pro.md %}">Population density analysis</a> shows where people are concentrated. Accessibility analysis asks whether they can reach what they need.

A coverage gap may call for a new facility, a street connection, or a transit change. A straight-line map can point that decision at the wrong place. A short trip on the network is also not proof of service quality.

## Distance-Based vs Network-Based Accessibility

Straight-line, or Euclidean, distance is the shortest line between two coordinates.

Euclidean distance = √[(x₂ − x₁)² + (y₂ − y₁)²]
{: .formula}

In a projected coordinate system that uses meters, the result is a ground distance. The same formula on longitude and latitude is not a walking distance. Even a correct straight-line distance ignores the street system. It crosses rivers, blocks, and closed roads as if they were open ground.

Network distance follows the connected links of a road, path, or transit network. It is the least-cost path. The cost, or impedance, may be length or travel time.

Network distance = the minimum sum of link costs along a connected path
{: .formula}

Travel time is often the more useful impedance. A short route through a congested center can take longer than a longer route on a faster street. Free-flow speed and peak-hour speed answer different questions, and the assumption should be stated.

Connectivity matters as much as length. The network has to know which links actually touch. An overpass should not connect to the road beneath it. A motorway without a crossing should not count as a walking link. If the graph is wrong, the path is wrong.

<figure>
  <img src="{{ '/assets/images/accessibility-network-comparison.svg' | relative_url }}" width="800" height="450" alt="A housing area and a clinic 300 meters apart in a straight line across a river. The street path uses a bridge and is much longer." loading="lazy" decoding="async">
  <figcaption>Illustration, not a mapped city. The straight line crosses the river. The walking path must use the bridge.</figcaption>
</figure>

Picture housing and a clinic 300 meters apart, with a river between them and the only bridge about 1.2 kilometers upstream. Each bank of the detour is about √(150² + 1,200²) meters, so the walk is on the order of 2.4 kilometers before the crossing itself. The straight line says the clinic is close. The street network says it is a substantial trip. Urban service accessibility that reports only the 300 meters misstates that neighborhood.

> A short straight-line distance is not evidence that a service is easy to reach.

## GIS Methods for Accessibility Analysis

ArcGIS Pro separates straight-line rasters, in Spatial Analyst, from paths along a network, in Network Analyst. The method has to match the question.

### Euclidean Distance Analysis

Euclidean distance writes a raster in which each cell stores the straight-line distance to the nearest source, such as a facility. It is a useful screen when no network exists yet. It is a poor final description of urban access wherever barriers and streets matter. Zonal statistics can average those cell values for a neighborhood. The summary inherits the limits of the straight-line surface. The course uses distance analysis and zonal statistics in this screening role.

### Network Analysis

A network dataset stores links with impedance, commonly distance, time, or both. The course builds networks from local data and from OpenStreetMap, with distance and time parameters. OpenStreetMap is a usable source and still needs checking. Missing paths, wrong one-way rules, and poor road classes pass directly into the result.

Optimal routing finds a least-cost path between two locations or across several zones. One route is not a picture of citywide coverage. That question needs service areas, a matrix, or a location model.

### Service Area Analysis

A service area is the region reachable from a facility within a cutoff, such as 10 minutes or 800 meters along the network. It describes service coverage outward from the facility. The cutoff has to match the mode and the planning standard. A five-minute drive and a fifteen-minute walk are different areas. Overlaps show redundancy. Gaps show places outside the cutoff. The polygon does not know whether the facility has capacity for the people inside it.

### Nearest Facility Analysis

Nearest facility analysis starts from an origin and finds the closest destinations along the network, with the path and the cost. It answers which station or hospital is actually nearest, rather than which symbol is nearest on the map. The course includes a case that locates police stations relative to a metro station. The result is a network cost, not a measured emergency response time.

### Origin-Destination Matrix

An origin-destination matrix stores travel cost between many origins and many destinations. A row might be a neighborhood and a column a hospital. The cell is time or distance on the network. The matrix is a table until a column is joined back to the origin areas and mapped.

A simple reading of that table is a cumulative count.

Cumulative access = the number of destinations whose travel cost is within the threshold
{: .formula}

### Location-Allocation

Location-allocation chooses sites from a list of candidates in order to serve demand. Demand is often population, taken from neighborhood or district counts of the kind discussed in density analysis. A common public-service objective is maximum population coverage within a travel cutoff. Other objectives minimize average travel or limit the number of facilities.

The course works through location-allocation for maximum population coverage, including an application in Milan. The model needs candidates, a demand representation, a network impedance, and a stated objective. Land price, building condition, and political limits are absent unless they are supplied as inputs. Urban facility planning can use the result as a scenario to check on the ground.

A gravity-style measure is related but different. One common form gives nearby opportunities more weight than distant ones:

Gravity-style access = Σ (facility size / travel cost<sup>β</sup>)
{: .formula}

β controls how fast access declines with cost. The course includes urban gravity modeling of goods and service flows on a network. The expression above is a standard form of the idea, not a claim that every exercise uses this exact equation.

<div class="table-wrap">
<table>
  <thead>
    <tr>
      <th scope="col">Method</th>
      <th scope="col">What it measures</th>
      <th scope="col">What it leaves out</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Euclidean distance</th>
      <td>Straight-line distance to the nearest source</td>
      <td>Streets, barriers, and one-way rules</td>
    </tr>
    <tr>
      <th scope="row">Route</th>
      <td>Least-cost path between locations</td>
      <td>Citywide coverage, unless many routes are compared</td>
    </tr>
    <tr>
      <th scope="row">Service area</th>
      <td>Area reachable within a cutoff</td>
      <td>Facility capacity and service quality</td>
    </tr>
    <tr>
      <th scope="row">Nearest facility</th>
      <td>Closest destinations along the network</td>
      <td>Whether that facility can accept the demand</td>
    </tr>
    <tr>
      <th scope="row">OD matrix</th>
      <td>Travel cost between many origins and destinations</td>
      <td>A map, until the table is joined back to areas</td>
    </tr>
    <tr>
      <th scope="row">Location-allocation</th>
      <td>Candidate sites that best serve demand</td>
      <td>Constraints that were not supplied as inputs</td>
    </tr>
  </tbody>
  <caption>Report the cutoff, the mode, and the impedance with the result.</caption>
</table>
</div>

## Practical Urban Applications

### Healthcare accessibility

Service areas show residents inside a travel standard. An origin-destination matrix compares neighborhoods. Location-allocation tests whether another set of sites would cover more people. None of these steps measures clinical quality.

### Educational facilities

Schools are often planned against a walking standard, so the graph should be a walking network. A straight buffer around a school will include children on the far side of a road that has no safe crossing.

### Emergency services

Nearest-facility and service-area models are a usual start for fire, ambulance, and police coverage. Speeds should reflect emergency travel if those rules can be encoded. Ordinary midday speeds do not describe a congested response.

### Public service coverage

Libraries, parks, and offices raise the same question: who falls inside a defined travel cost, and who does not? Overlap is not automatically waste. Some redundancy is a resilience choice. The map makes that overlap visible.

### Transportation planning

A single route is a path. A matrix is a picture of how zones are connected, including the travel consequence of a missing link or a new connection. Urban accessibility analysis using GIS is usually this system question, not one shortest path.

## How ArcGIS Pro Supports Urban Accessibility Analysis

The course follows the split between raster distance and network access. Distance analysis for urban services, zonal statistics, and reclassification screen and classify conditions before a network is required. Network lectures then cover construction from local data and from OpenStreetMap, optimal routing, service areas, nearest facilities, origin-destination matrices, a gravity model of flows, and location-allocation for population coverage. These network tasks are established Network Analyst problem types, apart from the gravity model, which uses network costs rather than a separate solver.

A practical order is: choose the mode, check the network, set the impedance, run the problem type that matches the question, and read the result against population. An unchecked connectivity rule is the step most likely to produce a confident and wrong map.

## Limitations and Data Considerations

- **Network completeness.** Missing paths change a walking analysis more than a driving analysis.
- **Connectivity.** Bridges, tunnels, and overpasses should connect only where a person can transfer.
- **Impedance.** Length ignores congestion. One speed ignores the peak. The assumption is part of the finding.
- **Demand geography.** Placing a neighborhood's population on one centroid hides residents at the edge, who face a different trip.
- **Facility attributes.** Capacity, hours, and eligibility are missing from a basic coverage model unless they are added.
- **Mode.** A car network does not describe walking or transit.
- **Currency.** A new road, a closure, or a moved clinic appears only after the data are updated.

A service-area polygon is a model of reach under stated rules. It is not a survey of whether residents consider the service accessible.

## Conclusion

Urban accessibility is the ease of reaching opportunities along ways people can travel. Straight-line distance is a screen, and it crosses barriers travelers cannot cross. Network distance and travel time follow the encoded roads or paths, and they depend on connectivity, impedance, and mode.

Service areas describe coverage. Nearest-facility analysis finds the closest real path. Origin-destination matrices compare many pairs. Location-allocation searches for sites that serve population under a chosen objective. Each one leaves out what it was not given. Stated with the cutoff, the network, and the demand unit, these tools describe access more honestly than a buffer drawn across the map.

## Related course

<a href="{{ site.course.url }}" target="_blank" rel="noopener noreferrer">{{ site.course.name }}</a> develops this topic through distance analysis, network construction with local data and OpenStreetMap, optimal routing, service areas, nearest facilities, origin-destination matrices, gravity modeling of flows, and location-allocation for population coverage. The course is in English and uses ArcGIS Pro. Programming is not required. ArcGIS Pro must be installed. Background in urban planning or geography is helpful but not mandatory.

<p class="cta-row">{% include course-cta.html label="Learn More: Urban Spatial Analysis with ArcGIS Pro" %}</p>
