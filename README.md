# Linked-adm-In
final project for the Advanced Data Mangaement course 

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings

### project description link
https://2024.aulaweb.unige.it/mod/page/view.php?id=56196

Nature of the Proposed Application

The proposed application is a job and industry relationship analysis tool. Its features and requirements include:

    Read/Write Intensity:
        The application is read-intensive, as it focuses on querying relationships (e.g., finding skills for a job or industries related to a company).
        It supports moderate write operations, including updates when new jobs, companies, or industries are added.

    Batch Processing:
        Batch imports are necessary during initialization or major updates to integrate new datasets into the graph database.

    Consistency and Availability:
        Eventual consistency suffices for non-critical updates to nodes and relationships, given the read-heavy workload.
        High availability is important for supporting real-time queries by users.

    Performance:
        Optimized for graph traversals like shortest path, recommendations, and neighborhood exploration.

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
