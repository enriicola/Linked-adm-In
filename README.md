# Linked-adm-In
final project for the Advanced Data Mangaement course 

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings

### project description link
https://2024.aulaweb.unige.it/mod/page/view.php?id=56196

# Nature of the Proposed Application

## The proposed application is a job and industry relationship analysis tool. Its features and requirements include:

Read/Write Intensity

    The application is predominantly read-intensive, focusing on querying relationships, such as finding the required skills for a job, the benefits offered, or the industries related to a company. This aligns with the domain's need for fast and frequent data lookups.
    Write operations are moderate, involving periodic updates when new jobs, companies, skills, or industries are added. Bulk writes may occur less frequently but require efficient batch processing mechanisms to handle larger datasets (e.g., importing new salary or employee count records).

Batch Processing

    Batch imports are critical during system initialization or when integrating significant updates to datasets, such as adding new companies, job listings, or benefit types.
    These imports should ensure referential integrity between nodes and relationships (e.g., ensuring a new job listing correctly links to the company and required skills).
    Using a pipeline or staged processing approach can optimize batch imports, allowing data verification and indexing before ingestion.

Consistency and Availability

    Eventual consistency is acceptable for non-critical updates (e.g., adding a new benefit to a job or updating a company's market value) as these updates do not impact the real-time querying experience.
    However, strong consistency may be required for critical operations such as creating or updating jobs where incomplete relationships (e.g., a job without a linked company or missing skills) could lead to data inconsistencies in user-facing queries.
    High availability is crucial to support real-time queries by users, particularly for applications requiring immediate responses, like job matching or skill recommendations.

Performance

    The system should be optimized for graph traversals, leveraging efficient querying techniques for:
        Exploring neighborhoods (e.g., finding all jobs associated with a specific skill or benefit).
        Recommendation tasks, such as identifying companies operating in similar industries or suggesting jobs requiring overlapping skillsets.
        Pathfinding, such as tracing relationships between a job and an industry through associated companies.
    Precomputing frequently accessed relationships (e.g., jobs requiring specific high-demand skills) or indexing key attributes (like Title, Name, or Level) can enhance query performance.
    Query optimization should prioritize reducing redundant traversals and filtering data early in the query pipeline.

Conceptual Schema

The following conceptual schema captures key entities and their relationships:

    Nodes:
        Job(Title, Expire date, Type): Represents a job posting (for now has a title attribute)
        Company(Name, Country, City, ZipCode, MarketValue): Represents a company (for now has a name attribute)
        IndustryDomain(Name): Represents an industry domain 
        Skill(Name, Level, Score): Represents skills associated with a job
        Benefit(Type (PK), Economical Value): Represents benefits offered for a job

    Relationships:
        BELONGS_TO: Links a job to an company (1-N: with an attribute "salary" of the association)
        --> the key of Job will be the composite of title and the foreign key of company
        REQUIRES: Links a job to the skills it demands (N-N)
        OFFERS: Links a job to the benefits it provides (N-N)
        OPERATES_IN: Links a company to the industries it operates in (1-N)

Queries:
    
    Query 1: Find all jobs that are Full-time and expiring within the next 30 days.
    
    Query 2: List companies in a specific city (e.g., "New York") with a market value greater than $1,000,000.
    
    Query 3: Retrieve all skills required for jobs offering benefits of a specific type (e.g., "401(k)") and having a score above 70.
    
    Query 4: List companies operating in a specific country (e.g., "USA") and associated with jobs that expire in more than 60 days.
    Workload: Relevant and Frequent Operations
    
    Query 5: Find all jobs of a specific type (e.g., "Internship") that require skills with a level of "Beginner" and are associated with companies in a particular city (e.g., "Chicago").
    
    Query 6: Retrieve all jobs, along with their required skills, benefits, and associated company details, for companies in cities with a zip code starting with "10".
    
    Query 7: List jobs requiring skills with a level of "Expert" and associated with companies operating in a specific domain (e.g., "Technology").
