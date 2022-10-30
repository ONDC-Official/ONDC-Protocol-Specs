# Revision History

|Version |Date |Changes
| :---: | :---: |  :--- |
| 0.4 |17th Sept 2022 |a. Added radius.unit (optional)
| 0.3 |5th Sept 2022 |a. Added holidays for stores
| 0.2 |3rd Sept 2022 |a. Added comments from participants
| 0.1 |2nd Sept 2022 |a. Created

# 1.Overview
The objective of this note is to propose a serviceability construct for retail providers (merchants) on ONDC. This serviceability construct will allow a provider to define the following:
- **Location constraint** - geographic boundaries within which the provider can fulfil orders. For hyperlocal, this is a construct based on lat/long coordinates such as circle, polygon (based on h3 / s2 cells) or any other universally referenceable GPS coordinate system, and for inter-city, derived based on pincodes. If no location constraint is defined for a merchant, it will be assumed that they are enabled for pan-India fulfillment;
- **Timing constraint** - store timings, if any, within which the provider accepts orders at their stores. If no timing constraints are defined, it will be assumed that the merchant accepts orders 24x7;

Most categories of products and services will be enabled for pan-India fulfilment unless the provider decides to restrict it through the serviceability construct. The serviceability construct will allow a buyer app to display catalogs, for only those stores, that can fulfil orders for a buyer, at a specific time, at their location.

# 2. Options
The ideal way to define the location constraints is to have a global geospatial referenceable index for a region on the map which defines the serviceability boundaries for the provider. Since this is not available, most technologies create squares (S2) or hexagons (h3) with indexes that are either predefined or dynamically generated.

Thus to define a region, around a specific location, in h3, we start with a hexagon around the geo-coordinates of the location at a granular resolution (say 10) and then polyfill the region around the hexagon. H3 libraries create multiple hexagons, of different resolutions, and some of these hexagons overlap. All the hexagons need to be above a specific resolution to allow set matching operations to determine whether an user, at a different location, is within the serviceability boundaries of this provider. This will result in a large number of h3 indexes, based on the size & type of the region.

# 3. Proposal
Since the protocol is optimized for data transfer between participants, the serviceability construct that needs to be communicated should be easy to configure for participants and be as lightweight as possible. Hence, the proposal is to use a circle (lat/long and radius) to define the construct for hyperlocal merchants. The inter-city serviceability construct will be defined at a later stage.

Define the existing data models in the protocol to define the serviceability construct as follows:

    a. Location constraint
Define the location constraint as a circle. The circle definition will only include the gps coordinates and radius and can be easily serializable & transmitted to the buyer app. The buyer app can check the distance between the gps coordinates of the buyer and the provider gps coordinates and determine if the prospective buyer is within the serviceability boundary.

Provider.locations.circle.gps = lat/long coordinates /* lat/long coordinates for store, same as Provider.locations.gps */

Provider.locations.circle.radius.unit = “km” /* unit of circle  - this is optional */

Provider.locations.circle.radius.value = “5” /* serviceability radius in unit (if provided) or km */

    b. Timing constraint
Define 2 different timing constraints - fixed timings and split timings (primarily for F&B restaurants). Either of these timing constraints may be used by providers to define their store timings.

**Fixed timings**
Provider.locations.time.days = “1,2,3,4,5,6,7”; /* comma separated values for day of week when store accepts orders; 1 - Monday till 7 - Sunday. If store is closed on Sunday, above will be “1,2,3,4,5,6” */

Provider.locations.time.range.start = “1100”; /use HHMM for start time/
Provider.locations.time.range.end = “2100”; /use HHMM for end time/
Provider.locations.time.schedule.holidays = [“2022-08-15”,”2022-08-19”];

**Split timings**
Provider.locations.time.days = “1,2,3,4,5,6,7”; /* comma separated values for day of week when store accepts orders; 1 - Monday till 7 - Sunday. If store is closed on Sunday, above will be “1,2,3,4,5,6” */

Provider.locations.time.schedule.frequency = “PT4H”; /ISO 8601 duration/
Provider.locations.time.schedule.times = [“1100”, “1900”]; /use HHMM for start time/
Provider.locations.time.schedule.holidays = [“2022-08-15”,”2022-08-19”];
