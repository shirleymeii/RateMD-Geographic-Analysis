# Project Readme

## A. Problem Context
Provide a brief description of the problem you're addressing. Include any background information necessary to understand the project.

Quality healthcare is a universal right for people across the United States. People often use online sources to evaluate and make a decision when choosing between medical professionals. Platforms like RateMD offer public reviews from patients which provide public reviews to those seeking medical attention. However, the availability and quality of physicians can vary signficiantly by geographic location. Some states, communities, and even zip codes see significant differences in the quality of healthcare. 

In the project, we will analyze doctor ratings on RateMD to identify patterns among highly rated and poorly rated physicians. We will also explore the distribution of physicians across specialities and evaluate whether there are enough physicians in each speciality. This way we can see whether certain specialities are underperforming. We will cross-reference this with geographic location and analyze whether there is a sufficient supply of quality doctors relative to population densities at the state, county and zip code level using U.S. Census population data.

We can also evaluate income levels in these geographic locations and compare it to the average physician rating and physicians available. We can explore whether income in geographic locations affect physician access and quality. This project will help in enabling decision making in opening public clinics and facilities.


## B. Requirements

### 1. Requirements Analysis
- Business Personas
  - Government Healthcare Officials: Identify undeserved communities and regions and record gaps in quality and accessible healthcare
  - Healthcare Physicians: Those looking to open a practice can identify where there may be a lack of accessible healthcare for specialties in certain areas. This can be a business opportunity for them 
  - Patients: Patients or families who may need more specialized medical attention when looking for places to move to can utilize the dashboard to see where there is more healthcare accessibility suitable for them

- Risks
  - Data Quality Issues: There may be duplicate doctor entries with the name slightly misspelled
  - Rating Bias: Online reviews can be fabricated or biased
 
- Timeline
  June 14th, 2026   Requirements Gathering, Cost, Benefits, Risks, Architecture
  June 21st, 2026 Data Modeling 
  June 26th, 2026, Data Pipeline Started
  July 3rd, 2026   Visualization Started
  
- Benefits
  - Identifies geographic areas with disproportionate healthcare accessibility and quality
  - Analyzes which specialties have higher ratings and which have lower ratings 
  - Aid in policy planning and healthcare facility management

### 2. Business Requirements
- Compare doctor ratings from RateMDs and identify patterns for high-rated physicians
- Determine whether there is an adequate number of doctors in a geographic area by referencing it with the population number
- Make information filterable by zip code, county, and state 

### 3. Functional Requirements
- Doctor Rating Analysis: Analyze doctor ratings from RateMD across average rating, number of reviews 
- Fuzzy Matching Pipeline must use fuzzy name matching to remove duplicates and standardize doctor names for data accuracy
- Geographic Analysis: Calculate and display the ratio of doctors in a county/state in proportion to the population. Further analyze income levels, the number and the quality of healthcare professionals
- Drill Down Reporting: Users should be able to toggle between county, state and zip code

### 4. Data Requirements
- RateMD Data: 
- US Census Data that provides population count and income information by zip code, county and state level

## C. Architecture

### 1. Information Architecture
Business processes supported:
- Evaluating physician quality and acessibility across U.S. geography (state/county/zip code level) 
- Identifiying underserved areas for healthcare policy planning and new clinic placement for healthcare officials and physicians
- Everyday individuals can preview healthcare accessibility across the U.S. for special healthcare needs

Analytics needed:
- Quality analytics that describe a doctor's average rating, rating count
- Ratio analytics: Doctor to population ratio by geography 
- Correlation analytics: healthcare accessibility and quality 

<img width="681" height="175" alt="Screenshot 2026-07-07 at 6 40 12 PM" src="https://github.com/user-attachments/assets/ef6ddacb-1f9b-405e-9d4f-b98d0a7f1632" />



### 2. Data Architecture
Sources: RateMD doctor ratings data that has the fields with their ID, name, speciality, rating average, rating count, rating regarding their helpfulness, punctuality, and knowledge. We will also utilize information about their practice location with postal code, city, and province. The data also covers doctors in Markham and Toronto, but for this solution, we will use U.S. Census data, so we will filter out doctors not based in the U.S. 
Ingestion: Stream the file via the public link, then process each complete doctor record one at a time. It will then be flattened with the location, rating, and doctor information fields and stored

<img width="990" height="257" alt="Screenshot 2026-06-25 at 5 59 31 PM" src="https://github.com/user-attachments/assets/e5bd061b-faad-4d19-acb6-483d33310182" />


### 3. Technical Architecture
<img width="838" height="212" alt="Screenshot 2026-07-07 at 6 39 04 PM" src="https://github.com/user-attachments/assets/900160c4-3c6a-4557-91dc-9e7c2bb73a59" />



### 4. Product Architecture
<img width="838" height="212" alt="Screenshot 2026-07-07 at 6 39 04 PM" src="https://github.com/user-attachments/assets/9d872243-1f54-4ca3-9515-72cffb7fa98d" />



## D. Modeling

For this project we use a star schema — the standard dimensional model for
analytics. A single central fact table holds the measurable values (the doctor
ratings), and it is surrounded by dimension tables that hold the descriptive
context (the doctor, their location, and their specialty). The fact table links to
each dimension through a foreign key. This structure is what makes the analysis fast
and intuitive: we can slice the ratings by any dimension and drill down through
geography (zip → county → state) to answer the project's questions.

The model is built around a clear grain: each row in the fact table represents
one doctor. Counting rows in the fact table therefore counts doctors, which is
what powers the "doctors per population" metric when combined with the population
figures stored in the geography dimension.

Fact Table

fact_doctor_rating — grain: one row per doctor.


Primary key: fact_rating_id
Measures: avg_rating, review_count, punctuality_rating,
helpfulness_rating, knowledge_rating
Foreign keys: doctor_id, geography_id, specialty_id


The measures are the numeric ratings we analyze. The foreign keys connect each
rating record to its descriptive dimensions.

Dimension Tables

dim_doctor — describes the individual physician.


doctor_id (PK), canonical_name
Holds the cleaned, fuzzy-matched doctor identity. The canonical_name is the
standardized name produced after de-duplicating near-identical entries
(e.g. "Dr. John Smith" and "John Smith MD").


dim_geography — describes the practice location and is the link to Census data.


geography_id (PK), zip_code, city, county, state, population, income
This is the conformed dimension that joins the RateMD ratings to U.S. Census
population and income. It carries the zip → county → state hierarchy, which enables
the drill-down requirement and the doctors-per-population analysis.


dim_specialty — describes the medical specialty.


specialty_id (PK), specialty_name, specialty_group
Lets us analyze ratings and doctor supply by specialty, and group related
specialties together (e.g. grouping sub-fields under a broader category).


How the model answers the requirements


Doctor rating analysis → the measures in fact_doctor_rating.
Doctors per population → count of fact rows joined to population in
dim_geography.
Filter/drill down by zip, county, state → the geography hierarchy in
dim_geography.
Specialty analysis → dim_specialty.


Field-type markers in the diagram: # = numeric, = = text, 🔑 = primary key,
↗ = foreign key.

<img width="1275" height="741" alt="star_schema_v2" src="https://github.com/user-attachments/assets/86d45061-a1db-4152-baeb-53192efedb49" />


## E. Methodology and Implementation
Describe the methodology used in the project and the steps followed during implementation.

This project was developed using an Agile, sprint-based methodology, organized
around the course milestones. Each sprint produced a working deliverable that the
next sprint built upon, allowing the three team members to work in parallel rather
than sequentially.




## F. Visualization
Provide details of the visualizations created for the project.

This project produced two Tableau visualizations analyzing doctor availability and quality across U.S. states.

**Visual 1 — Doctor Density Map:** A choropleth map of the U.S. shading each state by its total number of doctors, with a State → County → Zip drill-down hierarchy. Reveals where physicians are geographically concentrated (California, New York, and Texas hold the most).

**Visual 2 — Average Rating by State:** A horizontal bar chart of average doctor rating per state, filtered to reviewed doctors only and limited to the top 10 highest-rated states.

**Tool used:** Tableau Cloud, connected live to the Snowflake data warehouse.

**Tableau links:**
- Doctor Density Map:

<img width="1728" height="877" alt="Screenshot 2026-07-07 at 9 52 17 AM" src="https://github.com/user-attachments/assets/e2a485db-73d6-47d0-bc7f-b9e1efe42ba5" />

- Average Rating by State:

  <img width="1728" height="483" alt="Screenshot 2026-07-07 at 9 52 56 AM" src="https://github.com/user-attachments/assets/7e492ba8-daed-4738-adc3-f25733d9e7f9" />


## G. Insights
Highlight any key insights gained from the project.

The core finding from the analysis: doctor quantity does not equal doctor quality.

**Most doctors ≠ highest rated.** The states with the largest number of doctors — California, New York, and Texas — are not the highest-rated. Those states sit mid-pack on average rating, while smaller states like Utah, Idaho, and New Jersey lead in quality. A larger physician supply does not translate to better-reviewed care.

**Rating scale and the zero-rating exclusion.** Ratings in this dataset run on a roughly 0–1.3 raw scale. Doctors with an average rating of 0 were deliberately excluded from the analysis, because a 0 does not mean a doctor was rated poorly — it means the doctor had no reviews at all. Leaving them in was pulling the state averages down artificially and made good states look worse than they are. After removing the zero-review doctors, the meaningful comparison sits on a ~0–4 scale.

**Availability vs. quality are separate questions.** Combining the doctor-density map with the doctors-per-population ratio and the rating chart shows that a region can have many doctors, few doctors per capita, and mediocre ratings all at once — meaning "enough doctors" and "good doctors" have to be measured independently, which is exactly what the two visuals do.

**Why it matters.** For the project's stated users (health officials planning clinics, physicians choosing where to practice), this means raw doctor counts alone are misleading. Availability relative to population and rating quality both need to be on the map before any decision.

## H. Conclusion
Summarize the outcomes of the project and any potential next steps.

This project delivered a working analytics pipeline that answers two questions about U.S. healthcare: how good are the doctors, and are there enough of them relative to population.

**What was achieved.** We combined ~1.78 million RateMDs doctor-rating records with U.S. Census population and income data in a Snowflake data warehouse, modeled around a star schema (fact_doctor_rating surrounded by dim_doctor, dim_geography, and dim_specialty). On top of that we built Tableau visualizations: a doctor-density map with State → County → Zip drill-down, a top-10 average-rating-by-state chart, and a doctors-per-population availability metric. The result is a set of views that let a user evaluate both the quality and the availability of physicians, filterable by state, county, and zip.

**Key outcome.** The analysis showed that doctor quantity does not equal doctor quality — the states with the most doctors (California, New York, Texas) are not the highest-rated, while smaller states like Utah, Idaho, and New Jersey lead on ratings. Availability and quality are separate signals and both have to be measured.

**How the results can be used moving forward.** The dashboard supports real decisions: health officials can spot underserved areas for new clinic placement, physicians can identify regions lacking coverage in their specialty, and patients can compare healthcare access across locations. Future work could extend the analysis to income-vs-quality correlation, add specialty-level breakdowns, and incorporate more recent Census releases as they become available.


## I. References
- Provide a list of all references used in the project, formatted according to MLA style.

  1. RateMDs. *RateMDs — Doctor Reviews and Ratings*, RateMDs Inc., 2026, https://www.ratemds.com.
  2. Snowflake Inc. *Snowflake Documentation*, Snowflake Inc., 2026, https://docs.snowflake.com.

1. Author Last Name, First Name. *Title of Book*. Publisher, Year.
2. "Title of Article." *Name of Journal*, vol. 1, no. 1, Year, pp. 1-10.
3. *Title of Website*. Website Publisher, Year, URL.

---

*Replace placeholders like "path_to_image" with actual file paths or URLs.*
