# BirdsEye

BirdsEye is a real time navigation app that analyzes lane-level traffic flow along a user's route using live traffic camera feeds to give updates to the user.

## Inspiration

As a passionate group of four aspiring ML engineers, we came to ShellHacks with one goal: build something innovate, useful, and awesome. Soon after the competition began we found a goldmine of publicly available traffic video-streams provided by the Florida Department of Transportation (FDOT), and with some further research regarding common navigational API's, we decided upon an ambitious yet achievable 36-hour goal taking inspiration from Waymo's challenge to improve transportation. To empower drivers with a BirdsEye view of the road beyond the horizon, allowing them to understand the road miles ahead of then and make smarter driving decisions.

## What it does

Our hack provides the user a navigational interface similar to other popular navigation applications. However, what differentiates our project from many others is our implementation of truly live traffic analysis and lane suggestions. We use web-scrapers to collect the FDOT's publicly available traffic video-stream from the next three sequential cameras ahead of the user on their route. We then run a computer vision model to first identify the cars in the video, to then categorize which cars are driving in which lanes and estimate the speed of each car, after which we can compute an average speed for each lane at each traffic camera. We finally present this information to the user in our navigational app with a clear, clean, and intuitive UI.

## How we built it

We used Google Maps API to determine and display the route from a start and end point on the map. We extracted traffic camera data through the official FDOT website, streaming the video through ffmpeg and applied the YOLOv8 model (trained on the COCO dataset) to detect and track cars moving on a specific road. We incorporated metadata for each traffic camera to track cars entering and exiting a certain segment of the road for each lane, to track average speed and count of cars.

When defining the path, a list of cameras on the route is predetermined, and a sliding window of the next three cameras is applied while moving. At any given point, the next three cameras are then used to determine difference in lane speeds and how traffic compares across the three cameras (i.e. traffic is building up or clearing).

## Challenges we ran into

- Not enough compute - Running sequential computer vision models locally on our laptops introduced a lot of complexity due to the performance being so poor. To increase performance we had to make many tradeoffs that sacrificed accuracy for the performance. This could easily be solved by throwing a bigger computer at the issue.

- Missing Camera API: The lack of an active FDOT camera API forced us to improvise. We capture the live video by sniffing out the raw stream endpoints used by the FDOT website and piping them through ffmpeg. While functional, this process is inefficient and limits scalability. An official API would make this solution robust and production-ready.

## What's next for BirdsEye

FDOT Camera API: Our current method of accessing the traffic live-feeds is brutish, and requires a lot of manual work for each camera. We would love to be able to build or access an API that would make this process as seamless and reliable as the rest of our workflow.

Specialized CV models: Our hack was designed to be scalable, we could train models to recognize vehicles stopped in the emergency lanes, emergency vehicles, road hazards and debris, etc. The only limiting factor to what is possible is...

Compute: This was ultimately the biggest obstacle we had to deal with these last 36 hours given our limited technology. In the future BirdsEye would greatly benefit from the ability to leverage more compute to predict faster, smoother, and with greater accuracy.
