# Linked-adm-In
final project for the Advanced Data Mangaement course

### Assignment Steps:
- [X] 1. Propose a domain you are interested in and a relevant application for it (e.g.,e-commerce domain, shopping cart and session data management application). Take inspirations from datasets available online (e.g., on https://www.kaggle.com).
- [X] 2. Provide details about the nature of the proposed application (e.g., the application is read/write intensive, requires batch processing, ...), according to what discussed in the course and corresponding system requirements (e.g., eventual/strong consistency needed, high availability needed).
- [X] 3. Design a conceptual schema for the identified domain. The schema should include at least three associations.
- [X] 4. Identify a workload, i.e., a set of relevant and frequent operations, related to the chosen application. The workload should contain at least 5 structurally different operations. Describe each workload operation in natural language.
- [ ] 5. Use the aggregate-oriented design methodology (STEP 1-2-3) to design a set of aggregates for the domain and the workload at hand.
- [ ] 6. Design in MongoDB:
    - [ ] a. Design a schema for MongoDB (including partition keys and indexes), starting from (step 5), using the approaches/methodologies proposed in the course.
    - [ ] b. Specify each operation of the workload in the language supported by MongoDB
- [ ] 7. Design in Cassandra:
    - [ ] a. Design a schema for Cassandra  (including partition keys and indexes), starting from (step 5), using the approaches/methodologies proposed in the course.
    - [ ] b. Specify in CQL each operation of the workload.
- [ ] 8. Design in Neo4J:
    - [ ] a. Design a schema for Neo4j, using the approaches/methodologies proposed in the course.
    - [ ] b. Specify in Neo4j each operation of the workload.
- [ ] 9. Discuss which, among the three systems, is the most suitable to be used as  back-end for your application. Motivate your choice, taking into account the features of your application (step 2) and the identified workload (step 4). Let S be the selected system.
- [ ] 10. Provide details about the system configuration needed in system S  for storing/processing your data according to the chosen application.
- [ ] 11. Create the logical schema in system S.
- [ ] 12. Create an instance of your schema in the selected system, according to the logical schema just created. To this aim:
    - [ ] a. You can use either an already available dataset or a synthetic one but we encourage the first option (it might be difficult to synthetically generate a relevant dataset for your reference application). The dataset should have a reasonable size (few Mb).
    - [ ] b. Notice that selected datasets might need to be transformed in order to be used by your application. For dataset transformation, you can rely on either data transformation tools, such as Tableaux Prep (www.tableau.com), Apache Superset (superset.apache.org) Trifacta ( www.trifacta.com ), or other ETL tools such as Talend ( www.talend.com ), or scripts in your favorite language.
    - [ ] c. For importing datasets in the chosen system, you should refer to the available documentation for the system you have selected (e.g. https://www.datastax.com/dev/blog/simple-data-importing-and-exporting-with-cassandra for Cassandra and https://neo4j.com/developer/guide-importing-data-and-etl/perneo4J for Neo4J).
- [ ] 13. Implement the workload in system S.
- [ ] 14. Model in RDFS / OWL the main classes and the main properties corresponding to the entities and associations in the conceptual schema (step 3). In addition:
    - [ ] a. For each property, specify the corresponding domain and range.
    - [ ] b. Express which classes are equivalent and which ones are disjoint.
    - [ ] c. Specify (or add) at least an inverse property.
    - [ ] d. For all the modeled properties, specify whether they are functional (or inverse functional).
- [ ] 15. Model in RDF some instances  to populate your schema. In addition:
    - [ ] a. Relate instances to the corresponding class or property.
    - [ ] b. Clarify which individuals are identical and which ones are different.
- [ ] 16. Specify in SPARQL at least 3 queries to be executed over the defined RDF dataset. The requests should:
    - [ ] a. be structurally different (i.e., each of them should contain different constructs)
    - [ ] b. include at least one CONSTRUCT query
    - [ ] c. refer as much as possible to the requests included in the workload specified in PART II.
- [ ] 17. Check the correctness of the proposed RDF dataset, extended with RDFS /OWL constraints, and of the proposed SPARQL queries using RDF playground (http://rdfplayground.dcc.uchile.cl/) or any other RDF data store at your choice.

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings

### project description link
https://2024.aulaweb.unige.it/mod/page/view.php?id=56196

# Nature of the Proposed Application

## The proposed application is a job and industry relationship analysis tool. Its features and requirements include:
    
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

![ER1](https://github.com/user-attachments/assets/bc6fa2b4-81d7-491f-ab8b-dd604e804d6d)

Queries:
    
    Query 1: Find all jobs that are Full-time and expiring within the next 30 days.
    Q1(Job, [Job(type)_!, Job(expire_date)_!], [Job_!])
    
    Query 2: List the name of companies in New York with a market value greater than $1,000,000.
    Q2(Company, [Company(city)_!, Company(mv)_!], [Company(name)_!])
    
    Query 3: List name and market value of companies operating in Russia and associated with jobs that expire in more than 60 days.
    Workload: Relevant and Frequent Operations
    Q3(Job, [Company(country)_L, Job(expire_date)_!], [Company(name, mv)_L])
    
    Query 4: List type of jobs associated with companies operating in the Technology domain
    Q4(IndustryDomain, [IndustryDomain(Name)_!], [Job(type)_LO])

    Query 5: List type jobs associated with italian companies operating in the Technology domain
    Q5(Company, [IndustryDomain(Name)_O, Company(country)_!], [Job(type)_L])

    Query 6: Retrieve name of skills required for jobs offering benefits of type 401(k) and having a score above 70.
    Q6(Skill, [Skill(score)_!, Benefit(type)_OR], [Skill(name)_!])

        Query 7: Find the title of all jobs of type Internship that require skills with a level of "Beginner" and are associated with companies in Campobasso
    Q7(Job, [Job(type)_!, Skill(level)_R, Company(city)_L], [Job(title)_!])
