# Linked-adm-In
final project for the Advanced Data Mangaement course 

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings

### project description link
https://2024.aulaweb.unige.it/mod/page/view.php?id=56196

# Nature of the Proposed Application

## The proposed application is a job and industry relationship analysis tool. Its features and requirements include:

    Read/Write Intensity
    
    Read/Write Intensity
    The application is predominantly read-intensive, as it emphasizes retrieving data; such as querying the skills required for a job, the benefits offered, or the industries associated with a company.
    Write operations occur moderately, primarily when new jobs, companies, skills, or benefits are added to the system. These updates may include inserting related attributes like market values, skill scores, or job expiration dates.
    
    Batch Processing
    Batch imports can be essential only during initialization or when large updates to the dataset occur, such as integrating new company records, updated job postings, or expanded benefit types.
    Efficient batch processing should ensure data consistency, especially when dealing with related entities (e.g. associating a job with the correct company and required skills during import)
    
    Consistency and Availability
    Eventual consistency is sufficient for updates to non-critical attributes, such as adding new benefits or updating a company’s market value. These changes are not immediately critical for most queries.
    For core operations (e.g., adding new jobs or companies), ensuring consistency is more critical, as incomplete or incorrect relationships (e.g. a job missing its associated company) could affect query accuracy.
    However, having high availability clearly is the most important goal to reach, particularly for supporting real-time data retrieval and queries.
    
    Performance
    The system should be optimized for retrieving and filtering data (also across entities), such as:
    Searching for jobs based on attributes like type, location, or expiration date.
    Filtering companies by criteria like market value, location, or associated industries.

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
