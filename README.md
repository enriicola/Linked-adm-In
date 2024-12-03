# Linked-adm-In
final project for the Advanced Data Mangaement course 

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings

### project description link
https://2024.aulaweb.unige.it/mod/page/view.php?id=56196

Nature of the Proposed Application

The proposed application is a job and industry relationship analysis tool tailored for use with a Neo4j graph database. Its features and requirements include:

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
        Job: Represents a job posting.
        Company: Represents a company.
        Industry: Represents an industry domain.
        Skill: Represents a skill associated with a job.
        Benefit: Represents benefits offered for a job.

    Relationships:
        BELONGS_TO: Links a job to an industry.
        REQUIRES: Links a job to the skills it demands.
        OFFERS: Links a job to the benefits it provides.
        OPERATES_IN: Links a company to the industries it operates in.
        USES: Links a company to the skills it specializes in.

Workload: Relevant and Frequent Operations

    Find All Jobs in a Specific Industry:
        Description: Retrieve all job postings that belong to a specified industry.
        Query: Match jobs to industries via the BELONGS_TO relationship.

    Find Skills Required for a Job:
        Description: List all the skills associated with a specific job.
        Query: Match jobs to skills via the REQUIRES relationship.

    Identify Industries a Company Operates In:
        Description: Find all industries a specific company is linked to.
        Query: Match companies to industries via the OPERATES_IN relationship.

    Retrieve Benefits for a Specific Job:
        Description: List all the benefits offered for a particular job.
        Query: Match jobs to benefits via the OFFERS relationship.

    Recommend Jobs Based on Skills:
        Description: Suggest jobs that match a given list of skills, prioritizing those that match the most skills.
        Query: Match skills to jobs via the REQUIRES relationship and rank jobs based on skill matches.

