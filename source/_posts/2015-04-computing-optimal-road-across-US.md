---
layout: post
title: 寻找美国的最佳旅游线路
date: 2015/04/25 19:05:00
categories: 
- 旅行
tags: 
---

Last week, [Tracy Staedter](http://news.discovery.com/contributors/tracy-staedter.htm) from Discovery News proposed an interesting idea to me: Why not use the same algorithm from my [Where’s Waldo article](http://www.randalolson.com/2015/02/03/heres-waldo-computing-the-optimal-search-strategy-for-finding-waldo/) to compute the optimal road trip across every state in the U.S.? Visiting every U.S. state has long been on my bucket list, so I jumped on the opportunity and opened up my machine learning tool box for another quick weekend project.

### Planning the road trip

One of the hardest parts of planning a road trip is deciding where to stop along the way. Given how large and diverse the U.S. is, it’s especially difficult to make a road trip that will appeal to everyone. To stand a chance at making an interesting road trip, Tracy and I laid out a few rules from the beginning:

1. The trip must make at least one stop in all 48 states in the contiguous U.S.
2. The trip would only make stops at National Natural Landmarks, National Historic Sites, National Parks, or National Monuments.
3. The trip must be taken by car and never leave the U.S.

With those objectives in mind, Tracy compiled a list of 50 major U.S. landmarks — one in each state excluding Alaska/Hawaii and including D.C., and two in California. Tracy wrote about that process on Discovery News [here](http://news.discovery.com/tech/gear-and-gadgets/how-to-really-drive-across-the-us-hitting-all-major-landmarks-150309.htm).

The result was an epic itinerary with a mix of inner city exploration, must-see historical sites, and beautiful natural landscapes. All that was left was to figure out the path that would minimize our time spent driving and maximize our time spent enjoying the landmarks.

![mount_rushmore_pictures](https://ww3.sinaimg.cn/large/006tNc79gy1fcblpxy3d7j30sg0iz0xm.jpg)

Image credit: [Dean Franklin](http://commons.wikimedia.org/wiki/File:Dean_Franklin_-_06.04.03_Mount_Rushmore_Monument_(by-sa)-3_new.jpg)

### Computing the optimal road trip across the U.S.

With the list of landmarks in hand, the next step was to find the “true” distance between all of the landmarks by car. Since we can’t just drive a straight line between every landmark — driving by car has this pesky limitation of having to stay on roads — we needed to find the shortest route _by road_ between every landmark.

If you’ve ever used Google Maps to get the directions between two addresses, that’s basically what we had to do here. Except this time, we needed to look up 2,500 directions to get the “true” distance between all 50 landmarks — a monumental task if we had to do it by hand. Thankfully, the [Google Maps API](https://developers.google.com/maps/documentation/distancematrix/) makes this information freely available, so all it took was a short Python script to calculate the distance and time driven for all 2,500 routes between the 50 landmarks.

Now with the 2,500 landmark-landmark distances, our next step was to approach the task as a [traveling salesman problem](http://en.wikipedia.org/wiki/Travelling_salesman_problem): We needed to order the list of landmarks such that the total distance traveled between them is as small as possible if we visited them in order. This means finding the route that backtracks as little as possible, which is especially difficult when visiting Florida and the Northeast.

If you read my [Where’s Waldo article](http://www.randalolson.com/2015/02/03/heres-waldo-computing-the-optimal-search-strategy-for-finding-waldo/), you’re already aware of how difficult it can be to solve route optimization problems like this one. With 50 landmarks to put in order, we would have to exhaustively evaluate 3 x 10<sup>64</sup> possible routes to find the shortest one.

To provide some context: If you started computing this problem on your home computer right now, you’d find the optimal route in about 9.64 x 10<sup>52</sup> years — long after the Sun has entered its [red giant phase](http://en.wikipedia.org/wiki/Red_giant) and [devoured the Earth](http://en.wikipedia.org/wiki/Timeline_of_the_far_future#Future_of_the_Earth). This complication is why Google Map’s route optimization service only optimizes routes of up 10 waypoints, and the best free [route optimization service](http://www.routexl.com/) only optimizes 20 waypoints unless you pay them a lot of money to dedicate some bigger computers to it.

The traveling salesman problem is so notoriously difficult to solve that even [xkcd](http://xkcd.com/399/) poked fun at it:

![travelling_salesman_problem](https://ww1.sinaimg.cn/large/006tNc79gy1fcblpyrabwj30hs07v3zr.jpg)

Clearly, we need a smarter solution if we want to take this epic road trip in our lifetime. Thankfully, the traveling salesman problem has been [well-studied](http://en.wikipedia.org/wiki/Travelling_salesman_problem#Computing_a_solution) over the years and there are many ways for us to solve it in a reasonable amount of time.

If we’re willing to accept that we don’t need the _absolute best_ route between all of the landmarks, then we can turn to smarter techniques such as [genetic algorithms](http://en.wikipedia.org/wiki/Genetic_algorithm) to find a solution that’s good enough for our purposes. Instead of exhaustively looking at every possible solution, genetic algorithms start with a handful of random solutions and continually tinkers with these solutions — always trying something slightly different from the current solutions and keeping the best ones — until they can’t find a better solution any more.

I’ve included a visualization of a genetic algorithm solving a similar routing problem below.

### Road trip stopping at major U.S. landmarks

After less than a minute, the genetic algorithm reached a near-perfect solution that makes a complete trip around the U.S. in only 13,699 miles (22,046 km) of driving. I’ve mapped that route below.

Note: There’s an extra stop in Cleveland to force the route between Vermont and Michigan to stay in the U.S. rather than go through Canada. If you’re able to drive through Canada without issue, then take the direct route through Canada instead.

![best-road-trip-major-landmarks](https://ww3.sinaimg.cn/large/006tNc79gy1fcblq1fjwrj30sg0f8atd.jpg)

_Click [here](http://rhiever.github.io/optimal-roadtrip-usa/major-landmarks.html) for the interactive version_

Assuming no traffic, this road trip will take about 224 hours (9.33 days) of driving in total, so it’s truly an epic undertaking that will take at least 2-3 months to complete. The best part is that this road trip is designed so that you can start anywhere on the route as long as you follow it from then on. You’ll hit every major area in the U.S. on this trip, and as an added bonus, you won’t spend too long driving through the endless corn fields of Nebraska.

Here’s the Google Maps of the route: [[1]](https://www.google.com/maps/dir/Grand+Canyon+Village,+AZ+86023/Bryce+Canyon+National+Park,+Utah/Craters+of+the+Moon+National+Monument+%26+Preserve,+Arco,+ID/Yellowstone+National+Park/Pikes+Peak/Carlsbad+Caverns+National+Park,+New+Mexico/The+Alamo,+300+Alamo+Plaza,+San+Antonio,+TX+78205/Chickasaw+National+Recreation+Area,+1008+West+2nd+Street,+Sulphur,+OK+73086/Toltec+Mounds/Graceland,+3734+Elvis+Presley+Boulevard,+Memphis,+TN+38116/@36.6941176,-111.7228925,5z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x8733174f95ffe325:0xb8ccc2749a229ea1!2m2!1d-112.1401108!2d36.0544445!1m5!1m1!1s0x87356bc602c3eb2d:0x6be9d8fbbeac6d06!2m2!1d-112.18709!2d37.593038!1m5!1m1!1s0x54aa5f3acc0c2525:0x593b4ba65a2caf7b!2m2!1d-113.51665!2d43.41665!1m5!1m1!1s0x5351e55555555555:0xaca8f930348fe1bb!2m2!1d-110.588455!2d44.427963!1m5!1m1!1s0x8714a806033005bd:0xa67b8c79d6580c1e!2m2!1d-105.0422595!2d38.8408707!1m5!1m1!1s0x86e37913c4b8d75b:0x6ed43df7bd3bb2bd!2m2!1d-104.556714!2d32.147855!1m5!1m1!1s0x865c5f554d36895f:0x17f98f0f84e7c422!2m2!1d-98.486142!2d29.425967!1m5!1m1!1s0x87b32f3739269aaf:0x8e254781ad48873f!2m2!1d-97.012213!2d34.457043!1m5!1m1!1s0x87d2cf4a965885c3:0xc37ab5a2870636d2!2m2!1d-92.0651431!2d34.6470369!1m5!1m1!1s0x87d57d31cebd7225:0xfd48f1ff0c23cb11!2m2!1d-90.026049!2d35.047691!3e0) [[2]](https://www.google.com/maps/dir/Graceland,+3734+Elvis+Presley+Blvd,+Memphis,+TN+38116,+United+States/Vicksburg+National+Military+Park,+3201+Clay+Street,+Vicksburg,+MS+39183/French+Quarter,+New+Orleans,+LA/USS+Alabama,+2703+Battleship+Parkway,+Mobile,+AL+36603/Cape+Canaveral+Air+Force+Station,+Cape+Canaveral,+FL+32920/Okefenokee+Swamp+Park,+5700+Okefenokee+Swamp+Park+Road,+Waycross,+GA+31503/Fort+Sumter+National+Monument,+South+Carolina/Lost+World+Caverns,+HC+34+Box+308,+Lewisburg,+WV+24901/Wright+Brothers+National+Memorial,+1000+North+Croatan+Highway,+Kill+Devil+Hills,+NC+27948/George+Washingtons+Mount+Vernon,+3200+Mount+Vernon+Memorial+Highway,+Mount+Vernon,+VA+22121/@33.4772712,-87.9939648,6z/data=!3m2!4b1!5s0x89b7ae966f0cf157:0x12ec16b8acbc86b5!4m62!4m61!1m5!1m1!1s0x87d57d31cebd7225:0xfd48f1ff0c23cb11!2m2!1d-90.026049!2d35.047691!1m5!1m1!1s0x8628e1dbb2d1e605:0x47a0df70bc77d383!2m2!1d-90.84985!2d32.34655!1m5!1m1!1s0x8620a611ec349e7f:0x98bd3bbdbdb2223a!2m2!1d-90.0644107!2d29.9584426!1m5!1m1!1s0x889a4546cbb67455:0x2b8464983e59f46a!2m2!1d-88.014426!2d30.681803!1m5!1m1!1s0x88e0a4e74e6a8abb:0x2a16683cb4a44f!2m2!1d-80.572824!2d28.488672!1m5!1m1!1s0x88effa9000000001:0x3b673a65f4cf7886!2m2!1d-82.272327!2d31.056794!1m5!1m1!1s0x88fe76d4e9efb1c1:0xeb621a56355d9e74!2m2!1d-79.874692!2d32.752348!1m5!1m1!1s0x884c475a63a9d599:0x50a682db0de4ee63!2m2!1d-80.446765!2d37.832547!1m5!1m1!1s0x89a4e19dabec22e3:0x3df40e13de2a4632!2m2!1d-75.667738!2d36.014446!1m5!1m1!1s0x89b7ae9150022d97:0xa6efd58f6ac89b01!2m2!1d-77.086175!2d38.707982!3e0) [[3]](https://www.google.com/maps/dir/George+Washingtons+Mount+Vernon,+3200+Mt+Vernon+Memorial+Hwy,+Mt+Vernon,+VA+22121,+United+States/The+White+House/Maryland+State+House/City+of+New+Castle/Mid-Atlantic+Center+for+the+Arts+%26+Humanities/Liberty+Bell/Statue+of+Liberty+National+Monument/The+Mark+Twain+House+%26+Museum/The+Preservation+Society+of+Newport+County/USS+Constitution+Museum,+Building+22,+Charlestown+Navy+Yard,+Charlestown,+MA+02129/@40.5217266,-76.4189516,7z/data=!3m2!4b1!5s0x89e370f477285d8f:0xc1feffab81b5a250!4m62!4m61!1m5!1m1!1s0x89b7ae9150022d97:0xa6efd58f6ac89b01!2m2!1d-77.086175!2d38.707982!1m5!1m1!1s0x89b7b7bcdecbb1df:0x715969d86d0b76bf!2m2!1d-77.03653!2d38.897676!1m5!1m1!1s0x89b7f6502f6e80f7:0x3468f136f9ac32ce!2m2!1d-76.490974!2d38.978828!1m5!1m1!1s0x89c70370466deefb:0x2f863e3764f78ede!2m2!1d-75.564305!2d39.659749!1m5!1m1!1s0x89bf545bc303a301:0x27fa82c4186e8968!2m2!1d-74.914749!2d38.940329!1m5!1m1!1s0x89c6c8830b04502f:0xce39e053fb81ef23!2m2!1d-75.150282!2d39.94961!1m5!1m1!1s0x89c25090129c363d:0x40c6a5770d25022b!2m2!1d-74.0445!2d40.689249!1m5!1m1!1s0x89e65351cdec2249:0x5b86417eee8cecf!2m2!1d-72.701173!2d41.766759!1m5!1m1!1s0x89e5af5f65916565:0xb5b47e8cc238f4ea!2m2!1d-71.307317!2d41.474941!1m5!1m1!1s0x89e370f4778a4f39:0xa5c2057045470ca5!2m2!1d-71.055424!2d42.37398!3e0) [[4]](https://www.google.com/maps/dir/USS+Constitution+Museum,+Building+22,+Charlestown+Navy+Yard,+Charlestown,+MA+02129,+United+States/Acadia+National+Park,+Maine/Omni+Mount+Washington+Resort,+310+Mount+Washington+Hotel+Road,+Bretton+Woods,+NH+03575/Shelburne+Farms,+1611+Harbor+Road,+Shelburne,+VT+05482/Olympia+Entertainment,+2211+Woodward+Avenue,+Detroit,+MI+48201/Spring+Grove+Cemetery,+4521+Spring+Grove+Avenue,+Cincinnati,+OH+45232/Mammoth+Cave+National+Park,+1+Mammoth+Cave+Pkwy,+Mammoth+Cave,+KY+42259/West+Baden+Springs+Hotel,+8538+West+Baden+Avenue,+West+Baden+Springs,+IN+47469/Lincoln+Home+National+Historic+Site+Visitor+Center,+426+South+7th+Street,+Springfield,+IL+62701/@40.9225612,-88.4371122,5z/data=!3m1!4b1!4m56!4m55!1m5!1m1!1s0x89e370f4778a4f39:0xa5c2057045470ca5!2m2!1d-71.055424!2d42.37398!1m5!1m1!1s0x4caea3b20ea22925:0x62df220efc555584!2m2!1d-68.273335!2d44.338556!1m5!1m1!1s0x4cb39e6bbd3e1a7b:0x4e826063aba1f82b!2m2!1d-71.439586!2d44.257371!1m5!1m1!1s0x4cca7dad4648a66d:0xa0e49fd75e4ad683!2m2!1d-73.247278!2d44.395607!1m5!1m1!1s0x883b2d337c32f159:0xf25483a0b6175a31!2m2!1d-83.052681!2d42.33831!1m5!1m1!1s0x8841b4a1f5fa9487:0xfdc7e1db79a92d63!2m2!1d-84.524997!2d39.17433!1m5!1m1!1s0x8866024d94c97b57:0x1f88b3a06080a3c0!2m2!1d-86.100528!2d37.186998!1m5!1m1!1s0x886e9fab885e04c1:0xecd2e2daf9ad9b00!2m2!1d-86.618496!2d38.567008!1m5!1m1!1s0x887539c0fefd2895:0x3c7969f79156171c!2m2!1d-89.646184!2d39.797519!3e0) [[5]](https://www.google.com/maps/dir/Lincoln+Home+National+Historic+Site+Visitor+Center/The+Gateway+Arch/C.+W.+Parker+Carousel+Museum/Terrace+Hill/Taliesin/Historic+Fort+Snelling/Ashfall+Fossil+Beds+State+Historical+Park/Mount+Rushmore+National+Memorial/Fort+Union+Trading+Post+National+Historic+Site/Glacier+National+Park,+West+Glacier,+MT/@43.315618,-111.1846156,5z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x887539c0fefd2895:0x3c7969f79156171c!2m2!1d-89.646184!2d39.797519!1m5!1m1!1s0x87c0f0607541e5b5:0x79ed2889c696a834!2m2!1d-90.184776!2d38.624691!1m5!1m1!1s0x87c07b4135b40b6b:0x62eb2d0cca442e8e!2m2!1d-94.909536!2d39.317245!1m5!1m1!1s0x87ee98d8826c6059:0xf897fec1a497bdf4!2m2!1d-93.648542!2d41.583218!1m5!1m1!1s0x87fd7f2c067d4b79:0xfd8328ae06b1920f!2m2!1d-90.070467!2d43.141031!1m5!1m1!1s0x87f62eb7fe3f0211:0x792a64cfdded9ed6!2m2!1d-93.180627!2d44.89285!1m5!1m1!1s0x878538218f4f35b3:0xdc034379d751cc90!2m2!1d-98.158611!2d42.425!1m5!1m1!1s0x877d35d8b53ed6df:0xdaf53dbe055cc641!2m2!1d-103.459067!2d43.879102!1m5!1m1!1s0x5323f1c0457c38cf:0x31e2d875604a4461!2m2!1d-104.041483!2d48.00016!1m5!1m1!1s0x5368901555555555:0xaf16bc2215c55dec!2m2!1d-113.787022!2d48.759613!3e0) [[6]](https://www.google.com/maps/dir/Glacier+National+Park,+West+Glacier,+MT/Hanford+Site,+Benton+County,+WA/Columbia+River+Hwy,+Oregon/Cable+Car+Corner,+1099+Powell+Street,+San+Francisco,+CA+94108/San+Andreas+Fault/Hoover+Dam/Grand+Canyon+Village,+AZ/@41.4403108,-127.2327307,5z/data=!3m1!4b1!4m44!4m43!1m5!1m1!1s0x5368901555555555:0xaf16bc2215c55dec!2m2!1d-113.787022!2d48.759613!1m5!1m1!1s0x54985a3722b17805:0x350eb8746e4035a7!2m2!1d-119.488974!2d46.550684!1m5!1m1!1s0x54944389be165223:0x33164fd440f90961!2m2!1d-122.8660674!2d45.9751211!1m5!1m1!1s0x808580f32804fde1:0x2c3b1fc8b4028a4e!2m2!1d-122.410004!2d37.794634!1m5!1m1!1s0x80811b5d59ecac9b:0x2475ce107d37ac6f!2m2!1d-119.6755697!2d35.1361246!1m5!1m1!1s0x80c92b497f82a14b:0x89d59d0bd29de37!2m2!1d-114.737732!2d36.016066!1m5!1m1!1s0x8733174f95ffe325:0xb8ccc2749a229ea1!2m2!1d-112.1401108!2d36.0544445!3e0)

(Note that Google maps itself only allows 10 waypoints to be routed at a time, hence why there’s multiple Maps links.)

Here’s the full list of landmarks in order:

1. Grand Canyon, AZ
2. Bryce Canyon National Park, UT
3. Craters of the Moon National Monument, ID
4. Yellowstone National Park, WY
5. Pikes Peak, CO
6. Carlsbad Caverns National Park, NM
7. The Alamo, TX
8. The Platt Historic District, OK
9. Toltec Mounds, AR
10. Elvis Presley’s Graceland, TN
11. Vicksburg National Military Park, MS
12. French Quarter, New Orleans, LA
13. USS Alabama, AL
14. Cape Canaveral Air Force Station, FL
15. Okefenokee Swamp Park, GA
16. Fort Sumter National Monument, SC
17. Lost World Caverns, WV
18. Wright Brothers National Memorial Visitor Center, NC
19. Mount Vernon, VA
20. White House, Washington, DC
21. Colonial Annapolis Historic District, MD
22. New Castle Historic District, Delaware
23. Cape May Historic District, NJ
24. Liberty Bell, PA
25. Statue of Liberty, NY
26. The Mark Twain House & Museum, CT
27. The Breakers, RI
28. USS Constitution, MA
29. Acadia National Park, ME
30. Mount Washington Hotel, NH
31. Shelburne Farms, VT
32. Fox Theater, Detroit, MI
33. Spring Grove Cemetery, OH
34. Mammoth Cave National Park, KY
35. West Baden Springs Hotel, IN
36. Abraham Lincoln’s Home, IL
37. Gateway Arch, MO
38. C. W. Parker Carousel Museum, KS
39. Terrace Hill Governor’s Mansion, IA
40. Taliesin, WI
41. Fort Snelling, MN
42. Ashfall Fossil Bed, NE
43. Mount Rushmore, SD
44. Fort Union Trading Post, ND
45. Glacier National Park, MT
46. Hanford Site, WA
47. Columbia River Highway, OR
48. San Francisco Cable Cars, CA
49. San Andreas Fault, CA
50. Hoover Dam, NV

### Bonus: Road trip stopping at popular U.S. cities

If you’re more of a city slicker, the road trip above may not look very appealing to you because it involves spending a lot of time outdoors. But worry not, for I created a second road trip just for you! The road trip below stops at the TripAdvisor-rated [Best City to Visit](http://www.tripadvisor.com/Tourism-g191-United_States-Vacations.html) in every contiguous U.S. state.

Note: Again, there’s an extra stop in Cleveland to force the route between New Hampshire and Michigan to stay in the U.S. rather than go through Canada. If you’re able to drive through Canada without issue, then take the direct route through Canada instead. But really, Cleveland is a nice city to stop in (ranked #53 on TripAdvisor).

![best-road-trip-popular-cities](https://ww4.sinaimg.cn/large/006tNc79gy1fcblq515sjj30sg0fbnfm.jpg)

**Click [here](http://rhiever.github.io/optimal-roadtrip-usa/popular-cities.html) for the interactive version**

This road trip will more-or-less follow the same path as the major U.S. landmarks trip, covering a slightly shorter 12,290 mile (19,780 km) route around the U.S. Some larger states — like California and Texas — may have multiple cities you’d like to visit, so it’s probably worthwhile to stop at other larger cities along the route.

You may note that cities from North Dakota, Vermont, and West Virginia are missing. Out of the top 400 recommended cities to visit on TripAdvisor, none were from North Dakota, Vermont, nor West Virginia. This is especially interesting because TripAdvisor reviewers recommend cities like Flint, MI — the 7th most [crime-ridden city](http://www.neighborhoodscout.com/neighborhoods/crime-rates/top100dangerous/) in the U.S. — over any city in North Dakota, Vermont, and West Virginia. I’ll leave the interpretation of that fact to the reader.

Here’s the Google Maps of the route: [[1]](https://www.google.com/maps/dir/Oklahoma+City,+OK/Wichita,+KS/Denver,+CO/Albuquerque,+NM/Phoenix,+AZ/Las+Vegas,+NV/San+Francisco,+CA/Portland,+OR/Seattle,+WA/Boise,+ID/@40.1488805,-119.8298172,5z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x87ad8a547ef8d281:0x33a21274d14f3a9d!2m2!1d-97.5164276!2d35.4675602!1m5!1m1!1s0x87badb6ad27f182d:0x9396d5bf74d33d3e!2m2!1d-97.336111!2d37.688889!1m5!1m1!1s0x876b80aa231f17cf:0x118ef4f8278a36d6!2m2!1d-104.990251!2d39.7392358!1m5!1m1!1s0x87220addd309837b:0xc0d3f8ceb8d9f6fd!2m2!1d-106.609991!2d35.110703!1m5!1m1!1s0x872b12ed50a179cb:0x8c69c7f8354a1bac!2m2!1d-112.0740373!2d33.4483771!1m5!1m1!1s0x80beb782a4f57dd1:0x3accd5e6d5b379a3!2m2!1d-115.1398296!2d36.1699412!1m5!1m1!1s0x80859a6d00690021:0x4a501367f076adff!2m2!1d-122.4194155!2d37.7749295!1m5!1m1!1s0x54950b0b7da97427:0x1c36b9e6f6d18591!2m2!1d-122.6764816!2d45.5230622!1m5!1m1!1s0x5490102c93e83355:0x102565466944d59a!2m2!1d-122.3320708!2d47.6062095!1m5!1m1!1s0x54aef172e947b49d:0x9a5b989b36679d9b!2m2!1d-116.2146068!2d43.6187102!3e0) [[2]](https://www.google.com/maps/dir/Boise,+ID/Park+City,+UT/Jackson,+WY/Billings,+Mt/Sioux+Falls,+SD/Omaha,+NE/Des+Moines,+IA/Minneapolis,+MN/Milwaukee,+WI/Chicago,+IL/@42.8630089,-111.3939005,5z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x54aef172e947b49d:0x9a5b989b36679d9b!2m2!1d-116.2146068!2d43.6187102!1m5!1m1!1s0x87520f632c6303fd:0xd871c4df25375794!2m2!1d-111.4979729!2d40.6460622!1m5!1m1!1s0x53531a58fccf7f4b:0x3d1c01cbb13a835c!2m2!1d-110.7624282!2d43.4799291!1m5!1m1!1s0x53486f8888fa9d97:0x373556d4f179b550!2m2!1d-108.5006904!2d45.7832856!1m5!1m1!1s0x878eb498e0bdacd7:0xde95ff3aa8b2fccf!2m2!1d-96.7311034!2d43.5445959!1m5!1m1!1s0x87938dc8b50cfced:0x46424d4fae37b604!2m2!1d-95.9979883!2d41.2523634!1m5!1m1!1s0x87ee99a4c1611ee7:0x710028512691e4b2!2m2!1d-93.6091064!2d41.6005448!1m5!1m1!1s0x52b333909377bbbd:0x939fc9842f7aee07!2m2!1d-93.2650108!2d44.977753!1m5!1m1!1s0x880502d7578b47e7:0x445f1922b5417b84!2m2!1d-87.9064736!2d43.0389025!1m5!1m1!1s0x880e2c3cd0f4cbed:0xafe0a6ad09c0c000!2m2!1d-87.6297982!2d41.8781136!3e0) [[3]](https://www.google.com/maps/dir/Chicago,+IL/Indianapolis,+IN/Louisville,+KY/Columbus,+OH/Detroit,+MI/Cleveland,+OH/Manchester,+NH/Portland,+ME/Boston,+MA/Providence,+RI/@40.8703312,-83.683591,6z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x880e2c3cd0f4cbed:0xafe0a6ad09c0c000!2m2!1d-87.6297982!2d41.8781136!1m5!1m1!1s0x886b50ffa7796a03:0xd68e9df640b9ea7c!2m2!1d-86.158068!2d39.768403!1m5!1m1!1s0x88690b1ab35bd511:0xd4d3b4282071fd32!2m2!1d-85.7584557!2d38.2526647!1m5!1m1!1s0x883889c1b990de71:0xe43266f8cfb1b533!2m2!1d-82.9987942!2d39.9611755!1m5!1m1!1s0x8824ca0110cb1d75:0x5776864e35b9c4d2!2m2!1d-83.0457538!2d42.331427!1m5!1m1!1s0x8830ef2ee3686b2d:0xed04cb55f7621842!2m2!1d-81.6943605!2d41.49932!1m5!1m1!1s0x89e24ed668666ca3:0x6b0432461f357179!2m2!1d-71.4547891!2d42.9956397!1m5!1m1!1s0x4cb29c72aab0ee2d:0x7e9db6b53372fa29!2m2!1d-70.2553259!2d43.661471!1m5!1m1!1s0x89e3652d0d3d311b:0x787cbf240162e8a0!2m2!1d-71.0588801!2d42.3600825!1m5!1m1!1s0x89e444e0437e735d:0x69df7c4d48b3b627!2m2!1d-71.4128343!2d41.8239891!3e0) [[4]](https://www.google.com/maps/dir/Providence,+Rhode+Island/New+Haven,+CT/New+York,+NY/Ocean+City,+NJ/Philadelphia,+PA/Wilmington,+DE/Baltimore,+MD/Washington,+DC/Virginia+Beach,+VA/Charlotte,+NC/@38.4387905,-80.8583475,6z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x89e444e0437e735d:0x69df7c4d48b3b627!2m2!1d-71.4128343!2d41.8239891!1m5!1m1!1s0x89e7d8443a8070e5:0xf6a354c659b264ed!2m2!1d-72.9278835!2d41.308274!1m5!1m1!1s0x89c24fa5d33f083b:0xc80b8f06e177fe62!2m2!1d-74.0059413!2d40.7127837!1m5!1m1!1s0x89c0a53f45731825:0xbb986daa629826e7!2m2!1d-74.5746001!2d39.2776156!1m5!1m1!1s0x89c6b7d8d4b54beb:0x89f514d88c3e58c1!2m2!1d-75.1652215!2d39.9525839!1m5!1m1!1s0x89c70f185c46af6f:0x8516da5077308c00!2m2!1d-75.5397878!2d39.7390721!1m5!1m1!1s0x89c803aed6f483b7:0x44896a84223e758!2m2!1d-76.6121893!2d39.2903848!1m5!1m1!1s0x89b7c6de5af6e45b:0xc2524522d4885d2a!2m2!1d-77.0368707!2d38.9071923!1m5!1m1!1s0x89bac1e8fc1527a7:0x4161080a32e0173!2m2!1d-75.977985!2d36.8529263!1m5!1m1!1s0x88541fc4fc381a81:0x884650e6bf43d164!2m2!1d-80.8431267!2d35.2270869!3e0) [[5]](https://www.google.com/maps/dir/Charlotte,+NC/Charleston,+SC/Orlando,+FL/Atlanta,+GA/Nashville,+TN/Birmingham,+AL/Jackson,+MS/New+Orleans,+LA/Houston,+TX/Little+Rock,+AR/@32.2697109,-92.3880123,6z/data=!3m1!4b1!4m62!4m61!1m5!1m1!1s0x88541fc4fc381a81:0x884650e6bf43d164!2m2!1d-80.8431267!2d35.2270869!1m5!1m1!1s0x88fe7a42dca82477:0x35faf7e0aee1ec6b!2m2!1d-79.9310512!2d32.7764749!1m5!1m1!1s0x88e773d8fecdbc77:0xac3b2063ca5bf9e!2m2!1d-81.3792365!2d28.5383355!1m5!1m1!1s0x88f5045d6993098d:0x66fede2f990b630b!2m2!1d-84.3879824!2d33.7489954!1m5!1m1!1s0x8864ec3213eb903d:0x7d3fb9d0a1e9daa0!2m2!1d-86.7816016!2d36.1626638!1m5!1m1!1s0x888911df5885bfd3:0x25507409eaba54ce!2m2!1d-86.80249!2d33.5206608!1m5!1m1!1s0x86282b7f90741b21:0x713cde441f038a0!2m2!1d-90.1848103!2d32.2987573!1m5!1m1!1s0x8620a454b2118265:0xdb065be85e22d3b4!2m2!1d-90.0715323!2d29.9510658!1m5!1m1!1s0x8640b8b4488d8501:0xca0d02def365053b!2m2!1d-95.3698028!2d29.7604267!1m5!1m1!1s0x87d2a134a11f569b:0x3405f5100df35b17!2m2!1d-92.2895948!2d34.7464809!3e0) [[6]](https://www.google.com/maps/dir/Little+Rock,+AR/Branson,+MO/@35.6900438,-93.9392733,8z/data=!3m1!4b1!4m13!4m12!1m5!1m1!1s0x87d2a134a11f569b:0x3405f5100df35b17!2m2!1d-92.2895948!2d34.7464809!1m5!1m1!1s0x87cf01e9c1f60ea9:0xf3370960da92ac34!2m2!1d-93.2185144!2d36.6436731)

Here’s the full list of cities in order:

1. Oklahoma City, Oklahoma
2. Wichita, Kansas
3. Denver, Colorado
4. Albuquerque, New Mexico
5. Phoenix, Arizona
6. Las Vegas, Nevada
7. San Francisco, California
8. Portland, Oregon
9. Seattle, Washington
10. Boise, Idaho
11. Park City, Utah
12. Jackson, Wyoming
13. Billings, Montana
14. Sioux Falls, South Dakota
15. Omaha, Nebraska
16. Des Moines, Iowa
17. Minneapolis, Minnesota
18. Milwaukee, Wisconsin
19. Chicago, Illinois
20. Indianapolis, Indiana
21. Louisville, Kentucky
22. Columbus, Ohio
23. Detroit, Michigan
24. Cleveland, Ohio
25. Manchester, New Hampshire
26. Portland, Maine
27. Boston, Massachusetts
28. Providence, Rhode Island
29. New Haven, Connecticut
30. New York City, New York
31. Ocean City, New Jersey
32. Philadelphia, Pennsylvania
33. Wilmington, Delaware
34. Baltimore, Maryland
35. Washington, D.C.
36. Virginia Beach, Virginia
37. Charlotte, North Carolina
38. Charleston, South Carolina
39. Orlando, Florida
40. Atlanta, Georgia
41. Nashville, Tennessee
42. Birmingham, Alabama
43. Jackson, Mississippi
44. New Orleans, Louisiana
45. Houston, Texas
46. Little Rock, Arkansas
47. Branson, Missouri

### Make your own road trip

If you’d like to customize your own road trip, I’ve released the Python code I used in this project with an open source license and instructions for how to optimize your custom road trip. You can find the code [here](http://nbviewer.ipython.org/github/rhiever/Data-Analysis-and-Machine-Learning-Projects/blob/master/optimal-road-trip/Computing%20the%20optimal%20road%20trip%20across%20the%20U.S..ipynb).

### What about other parts of the world?

I’ve made another version for Europe [here](http://www.randalolson.com/2015/03/10/computing-the-optimal-road-trip-across-europe/). Check it out!

If you’d like to collaborate on making an optimal road trip for other parts of the world, feel free to [email me](http://randalolson.com/contact).

### Conclusions

The saying goes, “A journey of a thousand miles begins with a single step.” Really, that’s not true. Every major journey begins with a plan: where you’re going, where you’re stopping along the way, and how you’re getting there. I hope this article convinced you that machine learning can play a crucial role in that planning phase and save you a ton of time along the way.

Of course, it may not be practical for you to take a road trip of epic proportions like the ones described here. But really, this algorithm works just as well when you’re planning a smaller trip within your state as when you’re planning a larger trip spanning the entire world. All the algorithm needs are the distances travelled between every stop so it can compute the optimal route. How you get between those stops is up to you.

Happy road tripping!

ref:[http://www.randalolson.com/2015/03/08/computing-the-optimal-road-trip-across-the-u-s][1]

 [1]: http://www.randalolson.com/2015/03/08/computing-the-optimal-road-trip-across-the-u-s