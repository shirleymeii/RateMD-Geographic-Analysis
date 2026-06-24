# Project Readme

## A. Problem Context
Provide a brief description of the problem you're addressing. Include any background information necessary to understand the project.

Quality healthcare is a universal right for people across the United States. People often use online sources to evaluate and make a decision when choosing between medical professionals. Platforms like RateMD offer public reviews from patients which provide public reviews to those seeking medical attention. However, the availability and quality of physicians can vary signficiantly by geographic location. Some states, communities and even zip codes see significant differences in the quality of healthcare. 

In the project we will analyze doctor ratings on RateMD to identify patterns across highly rated physicians and lowly rated physicians. We will also explore the distribution of physicians in each speciality and evaluate if there are enough physicians in each speciality. This way we can see whether certain specialities are underperforming. We will cross reference this with geographic location and analyze whether there are an sufficient supply of quality doctors relative to population densities at the state, county and zip code level using U.S. Census population data.

We can also evaluate income levels in these geographic locations and compare it to the average physician rating and physicians available. We can explore whether income in geographic locations affect physician access and quality. This project will help in enabling decision making in opening public clinics and facilities.


This project will 
## B. Requirements

### 1. Requirements Analysis
- Business Personas
  - Government Healthcare Officials: Identify undeserved communities and regions and record gaps in quality and accessible healthcare
  - Healthcare Physicians: Those looking to open a practice can identify where there may be a lack of accessible healthcare for specialities in certain areas. This can be a business opportunity for them 
  - Patients: Patients or families who may need more specialized medical attention when looking for places to move to can utilize the dashboard to see where there is more healthcare accessibility suitable for them

- Risks
  - Data Quality Issues: There may duplicate doctor entries with the name slightly mispelled
  - Rating Bias: Online reviews can be fabricated or biased

- Costs
 
- Timeline
  June 14th, 2026   Requirements Gathering, Cost, Benefits, Risks, Architecture
  June 21st, 2026 Data Modeling 
  June 26th, 2026, Data Pipeline Started
  July 3rd, 2026   Visualization Started
  
- Benefits
  - Identifies geographic areas with disproportionate healthcare accessibility and quality
  - Analyzes which specialities have higher ratings and which have lower ratings 
  - Aid in policy planning and healthcare facility management

### 2. Business Requirements
- Compare doctor ratings from RateMDs and identify patterns for high rated physicians
- Determine whether there is an adequate number of doctors in a geographic area by referencing it with population number
- Make information filterable by zip code, county and state 

### 3. Functional Requirements
- Doctor Rating Analysis: Analyze doctor ratings from RateMD across average rating, number of reviews 
- Fuzzy Matching Pipeline must use fuzzy name matching to remove duplicates and standardize doctor names for data accuracy
- Geographic Analysis: Calculate and display the ratio of doctors in a county/state in proportion to the population. Further analyze income levels and number and quality of healthcare professionals
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
- Correlation analytics: Income level vs healthcare accessibility and quality 

<img width="891" height="227" alt="Screenshot 2026-06-24 at 7 12 45 PM" src="https://github.com/user-attachments/assets/39d8a320-ef32-477e-ac16-db8de850fb3c" />




### 2. Data Architecture
Sources: RateMD doctor ratings data that has the fields with their id, name, speciality, rating average, rating count, rating regarding their helpfulness, punctuality and knowledge. We will also utilize information about their practice location with postal code, city and province. The data also covers doctors in Markham and Toronto but in this solution we will be using U.S. Census data so we will filter out doctors not based in the U.S. 
Ingestion: Stream the file using the public link and then organizes each complete doctor record at a time. It will then be flattened with the location, rating and doctor information fields and stored

  - ![Data Architecture Diagram](path_to_image)

#### Medallion Architecture (if applicable)

- Stages:
  - **Bronze**: Extracting all the doctor information from the json data into a raw storage center with the fields sorted out. Once sorted out I have the information separated by each individual field by the doctor id. 
  - **Silver**: Cleaned, conformed, and enriched data.
  - **Gold**: Aggregated, business-ready data for analytics and reporting.
- Include a diagram if helpful.
  - ![Medallion Architecture Diagram](path_to_image)

### 3. Technical Architecture
- Define the software and hardware systems involved in the project.
- List any key technologies, tools, or platforms used. 
  - Example: 
    - Python for data analysis
    - Azure for cloud computing

### 4. Product Architecture
- Provide an overview of the product's overall structure.
- Include any major components and how they interact.
<img width="722" height="178" alt="Screenshot 2026-06-24 at 7 12 49 PM" src="https://github.com/user-attachments/assets/151104cc-7609-4d56-a11e-78ec11a5275b" />


## D. Modeling

### 1. Dimensional Modeling
- Explain the dimensional modeling
- Example:
  - **Facts**: describe all the facts
  - **Dimension**: include all dimensions

*Include any necessary images or diagrams to clarify the architecture.*
  - ![Dimensional Modeling Diagram](path_to_image)

## E. Methodology and Implementation
Describe the methodology used in the project and the steps followed during implementation.

- Outline the approach taken (e.g., Agile, Waterfall).
- Describe key phases, such as development, testing, deployment.
- Example:
  - Sprint 1: Setup and Data Collection
  - Sprint 2: Data Processing and Model Building
- Metadata Management
  - Data Dictionary
  - Mapping Sources and Target Systems
  - List of all functions
	- Function 1 
	- Function 2
	- Function 3
- ETL Extract Load Transform
- ELT Extract Transform Load
- Tools 

## F. Visualization
Provide details of the visualizations created for the project.

- Include charts, graphs, and any other visual representation of the data.
  - ![Visualization Example](path_to_image)
- Mention any libraries or tools used for visualization (e.g., Matplotlib, Power BI).

## G. Insights
Highlight any key insights gained from the project.

- Provide an overview of what was learned or discovered through data analysis.
- Example:
  - High correlation between customer satisfaction and response time.
  - Significant opportunity for cost reduction in supply chain operations.

## H. Conclusion
Summarize the outcomes of the project and any potential next steps.

- What was achieved?
- How can the results be used moving forward?
- Example:
  - The project successfully reduced costs by 20% through process automation.
  - Future work may include expanding the solution to new departments.

## I. References
- Provide a list of all references used in the project, formatted according to MLA style.

1. Author Last Name, First Name. *Title of Book*. Publisher, Year.
2. "Title of Article." *Name of Journal*, vol. 1, no. 1, Year, pp. 1-10.
3. *Title of Website*. Website Publisher, Year, URL.

---

*Replace placeholders like "path_to_image" with actual file paths or URLs.*
