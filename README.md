# Linked-adm-In
Final project for the Advanced Data Management course

### Assignment Steps:
- [X] 1. Propose a domain you are interested in and a relevant application for it (e.g.,e-commerce domain, shopping cart and session data management application). Take inspirations from datasets available online (e.g., on https://www.kaggle.com).
- [X] 2. Provide details about the nature of the proposed application (e.g., the application is read/write intensive, requires batch processing, ...), according to what discussed in the course and corresponding system requirements (e.g., eventual/strong consistency needed, high availability needed).
- [X] 3. Design a conceptual schema for the identified domain. The schema should include at least three associations.
- [X] 4. Identify a workload, i.e., a set of relevant and frequent operations, related to the chosen application. The workload should contain at least 5 structurally different operations. Describe each workload operation in natural language.
- [X] 5. Use the aggregate-oriented design methodology (STEP 1-2-3) to design a set of aggregates for the domain and the workload at hand.
- [x] 6. Design in MongoDB:
    - [x] a. Design a schema for MongoDB (including partition keys and indexes), starting from (step 5), using the approaches/methodologies proposed in the course.
    - [x] b. Specify each operation of the workload in the language supported by MongoDB
- [x] 7. Design in Cassandra:
    - [x] a. Design a schema for Cassandra  (including partition keys and indexes), starting from (step 5), using the approaches/methodologies proposed in the course.
    - [x] b. Specify in CQL each operation of the workload.
- [X] 8. Design in Neo4J:
    - [X] a. Design a schema for Neo4j, using the approaches/methodologies proposed in the course.
    - [X] b. Specify in Neo4j each operation of the workload.
- [X] 9. Discuss which, among the three systems, is the most suitable to be used as  back-end for your application. Motivate your choice, taking into account the features of your application (step 2) and the identified workload (step 4). Let S be the selected system.
- [X] 10. Provide details about the system configuration needed in system S  for storing/processing your data according to the chosen application.
- [X] 11. Create the logical schema in system S.
- [X] 12. Create an instance of your schema in the selected system, according to the logical schema just created. To this aim:
    - [X] a. You can use either an already available dataset or a synthetic one but we encourage the first option (it might be difficult to synthetically generate a relevant dataset for your reference application). The dataset should have a reasonable size (few Mb).
    - [X] b. Notice that selected datasets might need to be transformed in order to be used by your application. For dataset transformation, you can rely on either data transformation tools, such as Tableaux Prep (www.tableau.com), Apache Superset (superset.apache.org) Trifacta ( www.trifacta.com ), or other ETL tools such as Talend ( www.talend.com ), or scripts in your favorite language.
    - [X] c. For importing datasets in the chosen system, you should refer to the available documentation for the system you have selected (e.g. https://www.datastax.com/dev/blog/simple-data-importing-and-exporting-with-cassandra for Cassandra and https://neo4j.com/developer/guide-importing-data-and-etl/perneo4J for Neo4J).
- [X] 13. Implement the workload in system S.
- [X] 14. Model in RDFS / OWL the main classes and the main properties corresponding to the entities and associations in the conceptual schema (step 3). In addition:
    - [X] a. For each property, specify the corresponding domain and range.
    - [X] b. Express which classes are equivalent and which ones are disjoint.
    - [X] c. Specify (or add) at least an inverse property.
    - [X] d. For all the modeled properties, specify whether they are functional (or inverse functional).
- [X] 15. Model in RDF some instances  to populate your schema. In addition:
    - [X] a. Relate instances to the corresponding class or property.
    - [X] b. Clarify which individuals are identical and which ones are different.
- [X] 16. Specify in SPARQL at least 3 queries to be executed over the defined RDF dataset. The requests should:
    - [X] a. be structurally different (i.e., each of them should contain different constructs)
    - [X] b. include at least one CONSTRUCT query
    - [X] c. refer as much as possible to the requests included in the workload specified in PART II.
- [X] 17. Check the correctness of the proposed RDF dataset, extended with RDFS /OWL constraints, and of the proposed SPARQL queries using RDF playground (http://rdfplayground.dcc.uchile.cl/) or any other RDF data store at your choice.

### dataset link
https://www.kaggle.com/datasets/arshkon/linkedin-job-postings


# Nature of the Proposed Application

## (1-2) The proposed application is a job and industry relationship analysis tool. Its features and requirements include:
    
Read/Write Intensity

The application is predominantly read-intensive, as it emphasizes retrieving data; such as querying the skills required for a job, the benefits offered, or the industries associated with a company.
Write operations occur moderately, primarily when new jobs, companies, skills, or benefits are added to the system. These updates may include inserting related attributes like market values, skill scores, or job expiration dates.

Batch Processing

No Batch processing, beacause the only big import can be required during initialization: no large updates to the dataset will occur. 
The only updates might be occasionaly integrating new company records or updated job postings.

Consistency and Availability

Eventual consistency is sufficient for updates to non-critical attributes, such as adding new benefits or updating a company’s market value. These changes are not immediately critical for most queries.
For core operations (e.g., adding new jobs or companies), ensuring consistency is more critical, as incomplete or incorrect relationships (e.g. a job missing its associated company) could affect query accuracy.
However, having high availability clearly is the most important goal to reach, particularly for supporting real-time data retrieval and queries.

Performance

The system should be optimized for retrieving and filtering data (also across entities), such as:
Searching for jobs based on attributes like type, location, or expiration date.
Filtering companies by criteria like market value, location, or associated industries.

(3) Conceptual Schema

The following conceptual schema captures key entities and their relationships:

    Nodes:
        Job(Title, Expire date, Type): Represents a specifc job posting
        Company(Name, Country, City, ZipCode, MarketValue): Represents a company
        IndustryDomain(Name): Represents an industry domain 
        Skill(Name, Level, Score): Represents skills associated with a job
        Benefit(Type, Economical Value): Represents benefits offered for a job

    Relationships:
        BELONGS_TO: Links a job to an company 
        REQUIRES: Links a job to the skills it demands
        OFFERS: Links a job to the benefits it provides
        OPERATES_IN: Links a company to the industries it operates in

![Er1](https://github.com/user-attachments/assets/bb3e515b-c972-4ad3-827a-d7471e4cd96a)

(4) Queries:
    
    Query 1: Find all jobs that are Full-time and expiring within the next 30 days.
    Q1(Job, [Job(type)_!, Job(expire_date)_!], [Job_!])
    
    Query 2: List the name of companies in New York with a market value greater than $1,000,000.
    Q2(Company, [Company(city)_!, Company(mv)_!], [Company(name)_!])
    
    Query 3: List name and market value of companies operating in Russia and associated with jobs that expire in more than 60 days.
    Q3(Job, [Company(country)_L, Job(expire_date)_!], [Company(name, mv)_L])
    
    Query 4: List type of jobs associated with companies operating in the Technology domain
    Q4(IndustryDomain, [IndustryDomain(Name)_!], [Job(type)_LO])

    Query 5: List type jobs associated with italian companies operating in the Technology domain
    Q5(Company, [IndustryDomain(Name)_O, Company(country)_!], [Job(type)_L])

    Query 6: Retrieve name of skills required for jobs offering benefits of type 401(k) and having a score above 70.
    Q6(Skill, [Skill(score)_!, Benefit(type)_OR], [Skill(name)_!])

    Query 7: Find the title of all jobs of type Internship that require skills with a level of "Beginner" and are associated with companies in Hamburg
    Q7(Job, [Job(type)_!, Skill(level)_R, Company(city)_L], [Job(title)_!])
    
![ER2](https://github.com/user-attachments/assets/4ee6f063-1eb2-4135-add3-5e7ee2a707f9)

## (5) Aggregates

### JobOffer: <!-- Q1, Q3, Q7 -->
{
    <ins>title, companyName</ins>, expire_date, type, country, city, marketValue,
    requires: [{skill: {level}}]
}

### Company: <!-- Q2, Q5 -->
{
    <ins>name</ins>, marketValue, country, city,
    job_offers: [{job: {type}}],
    industryName
}

### IndustryDomain: <!-- Q4 -->
{
    <ins>name</ins>,
    operated: [{ company: [{ job: {type} }] }]
}

Note: dobule n-n relationship kept as list[lists] to semanthically keep the companies for further needs

### Skill: <!-- Q6 -->
{
    <ins>name</ins>,score,
    provides: [{benefit: {type}}] 
}

Note: double n-n relationship unpacked into a single list: we're only interested in benefits that skills provides, regardless of the jobs

## (6) Design in MongoDB 

### Queries associated with Skill: Q6
Selection attributes for Q6: {score, type} <!-- benefit type -->

The aggregate will remain the same...

### Skill: <!-- Q6 -->
{
    <ins>name</ins>,score,
    provides: [{benefit: {type}}]

Then MongoDB will add the _id:

{
    _id, <ins>name</ins>,score,
    provides: [{benefit: {type}}]
}

- A non-unique index on the partition key with Partition key = {score, type}
```
      db.skills.createIndex({ score: 1, "provides.benefit.type": 1 });
```
- A compound-unique index which contains the full shard key as a prefix of the index = {score, type, name}

Note: We know that MongoDB creates the index for the shardKey for you, but we're willing to be explicit in each step for clarity.
```
db.skills.createIndex(
          { score: 1, "provides.benefit.type": 1, name: 1 },
          { unique: true }
  );
```
- Shard collection
```
db.adminCommand({
  shardCollection: "db.skill",
  key: { score: 1, "provides.benefit.type": 1 }
});
```
- Q6:
```
  db.skills.find({score: {$gt: 70}, "provides.benefit.type": "401(k)"}, {name: 1, _id: 0});
```

Note: Remembering that there's no such a thing as "CREATE TABLE" in MongoDB we avoid reporting here insert commands of example data for the collection.

### Queries associated with IndustryDomain: Q4
Selection attributes for Q4: {name} <!-- IndustryDomain name -->

The aggregate will remain the same...

### IndustryDomain: <!-- Q4 -->
{
    <ins>name</ins>,
    operated: [{ company: [{ job: {type} }] }] <!-- dobule n-n relationship kept as list[lists] to semanthically keep the companies for further needs-->
}

Then MongoDB will add the _id:

{
    _id, <ins>name</ins>,
    operated: [{ company: [{ job: {type} }] }] <!-- dobule n-n relationship kept as list[lists] to semanthically keep the companies for further needs-->
}

- A unique index on the partition key, which is also the aggregate key = {name}
```
  db.industryDomains.createIndex({ name: 1 }, { unique: true });
```

- Shard collection
```
db.adminCommand({
  shardCollection: "db.industryDomain",
  key: { name: 1 }
});
```

- Q4:
```
  db.industryDomains.aggregate([
  {
    $match: {
      name: "Technology"
    }
  },
  {
    $unwind: "$operated"
  },
  {
    $unwind: "$operated.company"
  },
  {
    $unwind: "$operated.company.job"
  },
  {
    $project: {
      _id: 0,
      jobType: "$operated.company.job.type"
    }
  },
  {
    $group: {
      _id: null,
      jobTypes: { $addToSet: "$jobType" }
    }
  },
  {
    $project: {
      _id: 0,
      jobTypes: 1
    }
  }
]);
```

### Queries associated with Company: Q2, Q5
Selection attributes for Q2: {city, mv}

Selection attributes for Q5: {country, industryName} <!-- IndustryDomain name -->

The aggregate will remain the same...

### Company: <!-- Q2, Q5 -->
{
    <ins>name</ins>, marketValue, country, city,
    job_offers: [{job: {type}}],
    industryName <!-- simple attribute because it comes from a (1,1) association -->
}

Then MongoDB will add the _id:

{
    _id, <ins>name</ins>, marketValue, country, city,
    job_offers: [{job: {type}}],
    industryName <!-- simple attribute because it comes from a (1,1) association -->
}

- Given that the intersection between selection attributes in Q2 and Q5 is empty: instead of having a single non-unique index on {city, mv, country, industryName} we choose to separetely support both queries by creating two separate non-unique indexes onto them:
```
    db.companies.createIndex({ city: 1, mv: 1 });
    db.companies.createIndex({ country: 1, industryName: 1 });
```
Then, after having thought about both queries, we create a mixed non-unique index for sharding and to then support the uniqueness of name with a compound index:
```
    db.companies.createIndex({ city: 1, country: 1 });
```
- A compound-unique index which contains the full shard key as a prefix of the index = {score, type, name}
```
    db.companies.createIndex(
      { city: 1, country: 1, name: 1 },
      { unique: true }
    );
```
- Shard collection
```
    db.adminCommand({
      shardCollection: "db.Company",
      key: { city: 1, country: 1 }
    });
```
- Q2:
```
  db.companies.find({ city: "New York", mv: { $gt: 1000000 } }, { name: 1, _id: 0 });
```

- Q5:
```
  db.companies.aggregate([
  {
    $match: {
      country: "Italy",
      industryName: "Technology"
    }
  },
  {
    $unwind: "$job_offers"
  },
  {
    $project: {
      _id: 0,
      jobType: "$job_offers.job.type"
    }
  },
  {
    $group: {
      _id: null,
      jobTypes: { $addToSet: "$jobType" }
    }
  },
  {
    $project: {
      _id: 0, // the grouping creates again an _id :(
      jobTypes: 1
    }
  }
]);
```

### Queries associated with JobOffer: Q1, Q3, Q7
Selection attributes for Q1: {type, expire_date}

Selection attributes for Q3: {country, expire_date}

Selection attributes for Q7: {type, city, level}

The aggregate will remain the same...

### JobOffer: <!-- Q1, Q3, Q7 -->
{
    <ins>title, companyName</ins>, expire_date, type, country, city, marketValue,
    requires: [{skill: {level}}]
}

Then MongoDB will add the _id:

{
    _id, <ins>title, companyName</ins>, expire_date, type, country, city, marketValue,
    requires: [{skill: {level}}]
}

- We observe that type appears in both Q1 and Q7 while expire_date in both Q1 and Q3: so we have an empty intersection but a partial overlap that can be leveraged to support multiple queries efficiently.
Consequently, we can think about:
    - A composite index on {type, expire_date, city, level}: mongo uses a prefix for filtering so this will support both Q1 and Q7
    ```
    db.jobOffers.createIndex({ type: 1, expire_date: 1, city: 1, level: 1 });
    ```
    - We can follow a similar approach to combine Q1 and Q3 obtaining an index on {expire_date, type, country}
    ```
    db.jobOffers.createIndex({ expire_date: 1, type: 1, country: 1 });
    ```
    - Then, we think about having a compound unique index to enforce key constraint on the aggregate key {title, companyName}: given that type and expire_date are in partial overlap between queries we select them as a shard key. Here the unique index:
    ```
    db.jobOffers.createIndex(
      { type: 1, expire_date: 1, title: 1, companyName: 1 },
      { unique: true }
    );
    ``` 
- Shard collection
```
db.adminCommand({
  shardCollection: "db.JobOffer",
  key: { type: 1, expire_date: 1 }
});
```
- Q1:
```
  db.jobOffers.find(
  {
    type: "Full-time",
    expire_date: { $lte: new Date(new Date().setDate(new Date().getDate() + 30)) }
  },
  {
    _id: 0
  }
);
```

- Q3:
```
  db.jobOffers.aggregate([
  {
    $match: {
      country: "Russia",
      expire_date: { $gt: new Date(new Date().setDate(new Date().getDate() + 60)) }
    }
  },
  {
    $project: {
      _id: 0,
      companyName: 1,
      marketValue: 1
    }
  },
  {
    $group: {
      _id: "$companyName", // Group by company name
      marketValue: { $first: "$marketValue" } // Collect the first market value (one value per company!)
    }
  }
]);
```
Side note on this Q3: $first is used in MongoDB grouping to select the first encountered value of a field (e.g., marketValue) that is not part of the grouping key, providing a simple and efficient way to handle such fields without requiring computation.

- Q7:
```
  db.jobOffers.find(
  {
    type: "Internship",
    city: "Hamburg",
    "requires.skill.level": "Beginner"
  },
  {
    _id: 0,
    title: 1
  }
);
```

## (7) Design in Cassandra

### Queries associated with Skill: Q6
Selection attributes for Q6: {score, type} <!-- benefit type -->

Skill: <!-- Q6 -->
{
    <ins>name</ins>,score,
    provides: [{benefit: {type}}] <!-- double n-n relationship unpacked into a single list: we're only interested in benefits that skills provides, regardless of the jobs -->
}

- Given that we have only one query we can safely select {score, type} as partition key, while for the primary key we have to add the aggregate key "name" in order to have the aggregate identifier. We obtain:
    - Partition key = {score, type}
    - Primary key = {score, key, name} with name as clustering column
- Here are the CREATE commands in Cassandra:
```
  CREATE TYPE benefit_t (
    type text
);

CREATE TABLE Skills (
    name text,
    score int,
    type text,
    provides set<frozen<benefit_t>>,
    PRIMARY KEY ((score, type), name)
);

```
In summary:

TYPE benefit_t defines the structure for benefits for semantic reasons

((score, type)) as partition key ensures data is grouped by score and type

name as clustering column differentiates entries within each (score, type) partition

provides represents the benefits associated with a skill as a set of the previously created frozen benefit_t

- Q6:
```
SELECT name
FROM Skills
WHERE score > 70 AND type = '401(k)';
```

### Queries associated with IndustryDomain: Q4
Selection attributes for Q4: {name} <!-- IndustryDomain name -->

### IndustryDomain: <!-- Q4 -->
{
    <ins>name</ins>,
    operated: [{ company: [{ job: {type} }] }] <!-- dobule n-n relationship kept as list[lists] to semanthically keep the companies for further needs-->
}

- Given that name is both the only selection attribute and the aggregate key we only have to select it as Partition Key!
    - Partition key = {name}
- Here are the CREATE commands in Cassandra:
```
CREATE TYPE job_t (
    type text
);

CREATE TYPE company_t (
    job list<frozen<job_t>>
);

CREATE TABLE IndustryDomain (
    name text PRIMARY KEY,
    operated list<frozen<company_t>>
);
```
In summary:

TYPE job_t represents the type of jobs associated with a company

TYPE company_t represents a company, with its associated job types encapsulated as a list of job_t

IndustryDomain uses name as the Partition Key & the operated field stores the hierarchical relationship of companies and jobs.

- Q4:
```
SELECT operated 
FROM IndustryDomain 
WHERE name = 'Technology';
```

### Queries associated with Company: Q2, Q5
Selection attributes for Q2: {city, mv}

Selection attributes for Q5: {country, industryName} <!-- IndustryDomain name -->

### Company: <!-- Q2, Q5 -->
{
    <ins>name</ins>, marketValue, country, city,
    job_offers: [{job: {type}}],
    industryName <!-- simple attribute because it comes from a (1,1) association -->
}

- Unlikely the intersection between Q2 and Q5 selection attributes is empty: The only solution is to split the aggregate into two column-families!
```
CREATE TYPE job_t (
    type text
);

CREATE TABLE Company2 (
    city text,
    marketValue double,
    name text,
    country text,
    job_offers list<frozen<job_t>>,
    industryName text,
    PRIMARY KEY ((city, marketValue), name)
);

CREATE TABLE Company5 (
    city text,
    marketValue double,
    name text,
    country text,
    job_offers list<frozen<job_t>>,
    industryName text,
    PRIMARY KEY ((country, industryName), name)
);
```
Company2 will be used for executing Q2 and Company5 for executing Q5.

A further analysis can lead us to optimize, for example, C5 table by removing unnecessary fields (we still have C2 for other fields if required, so no needs to include extra field on a table that should be mainly tailored to allow the execution of query 5.
```
CREATE TABLE Company5 (
    country TEXT,
    industryName TEXT,
    name TEXT,
    job_offers LIST<FROZEN<job_t>>,
    PRIMARY KEY ((country, industryName), name)
);
```

- Q2:
```
SELECT name 
FROM Company2 
WHERE city = 'New York' AND marketValue > 1000000.0;
```
- Q5:
```
SELECT job_offers 
FROM Company5 
WHERE country = 'Italy' AND industryName = 'Technology';
```

### Queries associated with JobOffer: Q1, Q3, Q7
Selection attributes for Q1: {type, expire_date}

Selection attributes for Q3: {country, expire_date}

Selection attributes for Q7: {type, city, level}

### JobOffer: <!-- Q1, Q3, Q7 -->
{
    <ins>title, companyName</ins>, expire_date, type, country, city, marketValue,
    requires: [{skill: {level}}]
}

- Given our partial overlap we are able to carefully design this solution. Here are two candidate pairings:
    - Option 1: Combine Q1 and Q3
        Shared attribute: expire_date.
        Partitioning by expire_date allows efficient filtering for both queries.

    - Option 2: Combine Q1 and Q7
        Shared attribute: type.
        Partitioning by type allows efficient filtering for both queries.
  
--> We decide to opt for mixing Q1 and Q3 because expire_date is time-based and therefore with high selection factor (we have less types then expire_dates so makes sense to group the latter).
```
CREATE TYPE skill_t (
    level text
);

CREATE TABLE Job1_3 (
    expire_date DATE,
    type text,
    country text,
    title text,
    companyName text,
    city text,
    marketValue double,
    requires list<frozen<skill_t>>,
    PRIMARY KEY (expire_date, type, country, title, companyName)
);

CREATE TABLE Job7 (
    expire_date DATE,
    type text,
    country text,
    title text,
    companyName text,
    city text,
    marketValue double,
    requires list<frozen<skill_t>>,
    PRIMARY KEY ((type, city, level), title, companyName)
);
```
A further analysis can lead us to optimize, for example, Job7 table by removing unnecessary fields (we still have Job1_3 for other fields if required, so no needs to include extra field on a table that should be mainly tailored to allow the execution of query 7.
```
CREATE TABLE Job7 (
    type text,
    city text,
    level text,
    title text,
    companyName text,
    PRIMARY KEY ((type, city, level), title, companyName)
);
```

- Q1:
```
SELECT * 
FROM Job1_3 
WHERE type = 'Full-time' AND expire_date <= toDate(now()) + 30;
```
- Q3:
```
SELECT companyName, marketValue 
FROM Job1_3 
WHERE country = 'Russia' AND expire_date > toDate(now()) + 60;
```
- Q7:
```
SELECT title 
FROM Job7 
WHERE type = 'Internship' 
  AND city = 'Hamburg' 
  AND level = 'Beginner';
```

## (8) Design in Neo4J

![graph](https://github.com/user-attachments/assets/3680f296-22f0-4a47-8fa3-324540351316)

Query workload in Neo4J (from 1 to 7):

```
MATCH (j:Job)
WHERE j.type = 'Full-time'
  AND date(substring(j.exp_date, 0, 10)) <= date() + duration({days: 30})
RETURN j

MATCH (c:Company)
WHERE c.city = 'New York' AND c.mv > 1000000
RETURN c.name

MATCH (c:Company {country: 'Russia'})-[:LISTS]->(j:Job)
WHERE date(substring(j.exp_date, 0, 10)) <= date() + duration({days: 60})
RETURN DISTINCT c.name, c.mv

MATCH (id:Industry)<-[:OPERATES_IN]-(c:Company)-[:LISTS]->(j:Job)
WHERE id.name = 'Technology'
RETURN DISTINCT j.type

MATCH (id:Industry)<-[:OPERATES_IN]-(c:Company {country: 'Italy'})-[:LISTS]->(j:Job)
WHERE id.name = 'Technology'
RETURN DISTINCT j.type

MATCH (s:Skill)<-[:REQUIRES]-(j:Job)-[:OFFERS]->(b:Benefit)
WHERE s.score > 70 AND b.type = '401(k)'
RETURN DISTINCT s.name

MATCH (j:Job {type: 'Internship'})-[:REQUIRES]->(s:Skill {level: 'Beginner'}), 
      (j)<-[:LISTS]-(c:Company {city: 'Hamburg'})
RETURN j.title
```

Note: when it comes on choosing whether to put "WHERE" or leveraging "{...}" the decision relies on both flexibility and clarity. For complex conditions we chose WHERE, while for simple conditions of the "starting" node in the path we opted for a direct {...} inside the MATCH parenthesis.

## (9) System S choice:

Given that our application is:
    - predominanty read-intensive
    - with high avaibility constraints
    - some consistency constraints
    - no need of batch-processing
    - highly relational queries

- We select **Neo4J** as system S for suitable backend for our application. This is driven by the following reasonings:
    - The application is centered on relationships and traversals (e.g linking jobs to companies, industries, skills, and benefits), as graph databases benefits provide.
    - Cypher queries easily handles our workload, as opposed to MongoDB.
    - Our modelled graph is able to direclty reflect the conceptual schema, as opposed to Cassandra.
    - Perfectly suitable for high avaibility and read-intensive data
    - CA tailored
    
The only weakness is with respect to scalability of writes, but as we said the percentage of write operations is hugely lower than the write one (also, write operations are limited to little additions of new jobs when occasionaly they pop-up).

- We discarded MongoDB due to:
    - unintuitive relationships handling
    - not so optimized aggregation pipelines on most of the queries (due to them being highly relational)

- We discarded Cassandra due to:
    - Multiple denormalized tables, originated to a schema design that has to be tailored to specific queries (that present multiple disjoint scenarios with respect to selection attributes)
    - No query flexibility for future workload changes, highly probable in this dynamic world of jobs.

## (10) Neo4J: Configuration details

We need:
- Neo4J Aura instance to implement our system
- Query performance optimization by indexing when feasible (see (13))
- High RAM size for data traversal (Neo4J instance will be enough for our dataset's size)

If really implementing a scalable business solution, we would also need a clustering mechanisms to ensure high avaibility and causal consistency (let's remember that Neo4J is a CA system and it relies on causal consistency). Let's make an example configuration:

```
causal_clustering.enabled=true
causal_clustering.minimum_core_cluster_size_at_runtime=3
```
        
Then, when deploying Neo4j for a read-intensive workload with high availability and potential fault tolerance, the "R + W > N" rule becomes crucial: R (# of replicas that respond to read requests) and W (# of core nodes required for a successful write) must exceed the total number of N (core nodes in the cluster). 

Keeping in mind that in Neo4J each write operation is replicated across core nodes to keep consistency we have to carefully choose values for R, W and N.

- As said before N = 3 (so with a fault tolerance of 1 node failure and therefore a minimum quorum of 2 nodes)
- We can set W = 2 (at minimum 2 nodes have to commit a write for it being successfull)
  ```
  dbms.cluster.minimum_core_write_quorum=2
  ```
- We can set R = 2 (at least 2 replicas have to respond to that request)
  ```
  causal_clustering.read_replica_count=2
  ```
- R + W in this scenario = 4, that indeed is higher then N = 3!
    
Then, to scale-up in our read-intensive application, we would need to increase the read replicas.

For example, the following might be a feasible _job board_ scenario, in which the course concepts could be highly applied:

To scale up in our read-intensive application we would primarily focus on increasing the number of read replicas, which in Neo4j are dedicated nodes that serve only read queries, offloading the weight from the core nodes and allowing the system to handle significantly higher query volumes.

For example, in our job-board scenario mentioned above, the system must handle a high volume of user queries. Since the majority of these operations are read-only, the read replicas can efficiently handle these requests without impacting the performance of the core nodes.

So, in our previous example, as the number of users grows, we can step-by-step increase the number of read replicas depending on the query load; and since read replicas only need to synchronize with the core nodes periodically they can scale horizontally (and also being geographically replicated) without requiring changes to the core cluster configuration!

## (11) Neo4J: Schema details

Given Neo4J schema-less nature we don't have to provide any schema details (like in Cassandra for example, where we would have needed some table specifications). Here we just need to dynamically create nodes and their relationships, as we'll do in the next step.
For simplicity, Jobs will be considered as merely identified by their title.
Nevertheless, here follows a basic **example** on how the nodes should appear in our database insertion:

```
CREATE (:Industry {name: 'Data management'});
CREATE (:Company {name: 'Unige', mv: 'Data management', country: 'Italy', city: 'Genova', zipcode: '16100'});
CREATE (:Job {type: 'Full-time', exp_date: '2025-01-01', title: 'Pitch creator'});
CREATE (:Skill {name: 'Video editor', level: 'Advanced', score: 85});
CREATE (:Benefit {type: 'mark', ev: '110L'});

MATCH (j:Job {title: 'Pitch creator'}), 
      (c:Company {name: 'Unige'}), 
      (i:Industry {name: 'Data management'}), 
      (s:Skill {name: 'Video editor'}), 
      (b:Benefit {type: 'mark'})
CREATE (c)-[:LISTS]->(j);
CREATE (j)-[:OPERATES_IN]->(i);
CREATE (j)-[:REQUIRES]->(s);
CREATE (j)-[:OFFERS]->(b);
CREATE (c)-[:OPERATES_IN]->(i);
```

## (12) Neo4J: Graph details

We started from the linkedin-job-postings database and we continued by transforming it to meet our needs (removing unused fields and adding market value & economical value for queries enrichment).

These are our dataset links:
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/company.csv
```
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/job.csv
```
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/benefit.csv
```
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/industry.csv
```
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/skill.csv
```
```
https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/relationships.csv
```
These are our Cypher scripts:
```
:param {
  // Define the file path root and the individual file names required for loading.
  // https://neo4j.com/docs/operations-manual/current/configuration/file-locations/
  file_path_root: '[file:///](https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/data/)',
  file_0: 'benefit.csv',
  file_1: 'company.csv',
  file_2: 'job.csv',
  file_3: 'industry.csv',
  file_4: 'skill.csv',
  file_5: 'relationships.csv'
};

// CONSTRAINT creation
// -------------------
//
// Create node uniqueness constraints, ensuring no duplicates for the given node label and ID property exist in the database. This also ensures no duplicates are introduced in future.
//
// NOTE: The following constraint creation syntax is generated based on the current connected database version 5.26-aura.
CREATE CONSTRAINT title_Job_uniq IF NOT EXISTS
FOR (n: Job)
REQUIRE (n.title) IS UNIQUE;
CREATE CONSTRAINT name_Company_uniq IF NOT EXISTS
FOR (n: Company)
REQUIRE (n.name) IS UNIQUE;
CREATE CONSTRAINT name_Industry_uniq IF NOT EXISTS
FOR (n: Industry)
REQUIRE (n.name) IS UNIQUE;
CREATE CONSTRAINT type_Benefit_uniq IF NOT EXISTS
FOR (n: Benefit)
REQUIRE (n.type) IS UNIQUE;
CREATE CONSTRAINT name_Skill_uniq IF NOT EXISTS
FOR (n: Skill)
REQUIRE (n.name) IS UNIQUE;

:param {
  idsToSkip: []
};

// NODE load
// ---------
//
// Load nodes in batches, one node label at a time. Nodes will be created using a MERGE statement to ensure a node with the same label and ID property remains unique. Pre-existing nodes found by a MERGE statement will have their other properties set to the latest values encountered in a load file.
//
// NOTE: Any nodes with IDs in the 'idsToSkip' list parameter will not be loaded.
LOAD CSV WITH HEADERS FROM ($file_path_root + $file_2) AS row
WITH row
WHERE NOT row.title IN $idsToSkip AND NOT row.title IS NULL
CALL {
  WITH row
  MERGE (n: Job { title: row.title })
  SET n.title = row.title
  SET n.type = row.type
  SET n.exp_date = row.exp_date
} IN TRANSACTIONS OF 10000 ROWS;

LOAD CSV WITH HEADERS FROM ($file_path_root + $file_1) AS row
WITH row
WHERE NOT row.name IN $idsToSkip AND NOT row.name IS NULL
CALL {
  WITH row
  MERGE (n: Company { name: row.name })
  SET n.name = row.name
  SET n.city = row.city
  SET n.country = row.country
  SET n.zipcode = toFloat(trim(row.zipcode))
  SET n.mv = toInteger(trim(row.mv))
} IN TRANSACTIONS OF 10000 ROWS;

LOAD CSV WITH HEADERS FROM ($file_path_root + $file_3) AS row
WITH row
WHERE NOT row.name IN $idsToSkip AND NOT row.name IS NULL
CALL {
  WITH row
  MERGE (n: Industry { name: row.name })
  SET n.name = row.name
} IN TRANSACTIONS OF 10000 ROWS;

LOAD CSV WITH HEADERS FROM ($file_path_root + $file_0) AS row
WITH row
WHERE NOT row.type IN $idsToSkip AND NOT row.type IS NULL
CALL {
  WITH row
  MERGE (n: Benefit { type: row.type })
  SET n.type = row.type
  SET n.ev = toInteger(trim(row.ev))
} IN TRANSACTIONS OF 10000 ROWS;

LOAD CSV WITH HEADERS FROM ($file_path_root + $file_4) AS row
WITH row
WHERE NOT row.name IN $idsToSkip AND NOT row.name IS NULL
CALL {
  WITH row
  MERGE (n: Skill { name: row.name })
  SET n.name = row.name
  SET n.level = row.level
  SET n.score = toInteger(trim(row.score))
} IN TRANSACTIONS OF 10000 ROWS;


// RELATIONSHIP load
// -----------------
//
// Load relationships in batches, one relationship type at a time. Relationships are created using a MERGE statement, meaning only one relationship of a given type will ever be created between a pair of nodes.
LOAD CSV WITH HEADERS FROM ($file_path_root + $file_5) AS row
WITH row 
CALL {
  WITH row
  MATCH (source: Job { title: row.from })
  MATCH (target: Skill { name: row.to })
  MERGE (source)-[r: REQUIRES]->(target)
} IN TRANSACTIONS OF 10000 ROWS;

LOAD CSV WITH HEADERS FROM ($file_path_root + $file_5) AS row
WITH row 
CALL {
  WITH row
  MATCH (source: Company { name: row.from })
  MATCH (target: Job { title: row.to })
  MERGE (source)-[r:
```

We noted that, when loading the last CSV file (for the relationships), Neo4J was very slow due to its unfeasability on handling batch processing.

## (13) Neo4J: Workload implementation

- Here follows the workload implementation on Neo4J together with their execution explanation. We also decided to show some meaninfgul results with some indexes that could enhance systems' capabilities.
   
Once Neo4j has located the starting node, traversals through relationships are very efficient (O(1) basically). However... finding the starting node in a query is where indexes come into play! 

```
MATCH (j:Job)
WHERE j.type = 'Full-time'
  AND date(substring(j.exp_date, 0, 10)) <= date() + duration({days: 30})
RETURN j
```

![1 1](https://github.com/user-attachments/assets/8957184d-45be-4ed0-8dfb-08ed394270a4)

---------------------------------------------------------------------------------

```
MATCH (c:Company {country: 'Russia'})-[:LISTS]->(j:Job)
WHERE date(substring(j.exp_date, 0, 10)) <= date() + duration({days: 60})
RETURN DISTINCT c.name, c.mv
```

![1 2](https://github.com/user-attachments/assets/a63731d5-9965-4008-95c7-53ce3001dfeb)

---------------------------------------------------------------------------------

```
MATCH (j:Job {type: 'Internship'})-[:REQUIRES]->(s:Skill {level: 'Beginner'}), 
      (j)<-[:LISTS]-(c:Company {city: 'Hamburg'})
RETURN j.title
```

![1 3](https://github.com/user-attachments/assets/3ff1e947-fcee-4285-bde7-3dfc64d6f64c)

---------------------------------------------------------------------------------

Let's put some indexes:
```
CREATE INDEX FOR (j:Job) ON (j.type);
CREATE INDEX FOR (j:Job) ON (j.exp_date);
```

The first query changed its execution plan in the following way, while the other two remained unchanged. In our opinion this can be related to the fact that in the first query the index matched with all the selection attributes.

![1 1_idx](https://github.com/user-attachments/assets/2d0a92b3-67e1-49ef-981c-9f5de6e1a473)

---------------------------------------------------------------------------------

```
MATCH (c:Company)
WHERE c.city = 'New York' AND c.mv > 1000000
RETURN c.name
```

![2 1](https://github.com/user-attachments/assets/6e9239c4-1ce4-4519-88ca-d60c6a996472)

---------------------------------------------------------------------------------

```
MATCH (id:Industry)<-[:OPERATES_IN]-(c:Company {country: 'Italy'})-[:LISTS]->(j:Job)
WHERE id.name = 'Technology'
RETURN DISTINCT j.type
```

![2 2](https://github.com/user-attachments/assets/29e3938e-0787-41de-850c-91ecb4278164)

---------------------------------------------------------------------------------

Let's put some indexes:
```
CREATE INDEX FOR (c:Company) ON (c.city);
CREATE INDEX FOR (c:Company) ON (c.mv);
CREATE INDEX FOR (c:Company) ON (c.country);
```

The first query changed its execution plan in the following way, while the other one remained unchanged. In our opinion this can be related to the fact that in the second query the automatically created unique index on the primary key field "name" was indeed used (so no need to use the new ones).

![2 1_2](https://github.com/user-attachments/assets/cade8357-bfc9-4583-9b2b-1a6ff8bc29ff)

---------------------------------------------------------------------------------

```
MATCH (id:Industry)<-[:OPERATES_IN]-(c:Company)-[:LISTS]->(j:Job)
WHERE id.name = 'Technology'
RETURN DISTINCT j.type
```

![3](https://github.com/user-attachments/assets/ca42b111-7e40-4cb3-884d-90789a7b2f26)

---------------------------------------------------------------------------------

In this case the following index already exists by Neo4J, for the reasons explained before.
```
CREATE INDEX FOR (id:IndustryDomain) ON (id.name);
```

---------------------------------------------------------------------------------

```
MATCH (s:Skill)<-[:REQUIRES]-(j:Job)-[:OFFERS]->(b:Benefit)
WHERE s.score > 70 AND b.type = '401(k)'
RETURN DISTINCT s.name
```

![4](https://github.com/user-attachments/assets/fe68911c-01a4-465a-b7fa-60e8634c8f80)

---------------------------------------------------------------------------------

Let's put some indexes:
```
CREATE INDEX FOR (s:Skill) ON (s.score);
CREATE INDEX FOR (s:Skill) ON (s.level);
```

Again, these indexes are not used due to the benefit.type unique index being already present.

---------------------------------------------------------------------------------

## (14) Model in RDFS / OWL the main classes and properties

a. (Specify classes and properties) + for each property, specify the corresponding domain and range.

For simplicity, Jobs will be considered as merely identified by their title.

```
@prefix ex: <http://example.org/schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

# Classes:

ex:Job a rdfs:Class .
ex:Company a rdfs:Class .
ex:IndustryDomain a rdfs:Class .
ex:Skill a rdfs:Class .
ex:Benefit a rdfs:Class .

# (Subject to Object) Properties:

ex:belongsTo a rdf:Property ; rdfs:domain ex:Job ; rdfs:range ex:Company .
ex:requires a rdf:Property ; rdfs:domain ex:Job ; rdfs:range ex:Skill .
ex:offers a rdf:Property ; rdfs:domain ex:Job ; rdfs:range ex:Benefit .
ex:operatesIn a rdf:Property ; rdfs:domain ex:Company ; rdfs:range ex:IndustryDomain .

# (Subject to Literal) Properties

# Job
ex:jobTitle a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:string .

ex:jobType a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:string .

ex:expireDate a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:date .

# Company
ex:companyName a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string .

ex:country a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string .

ex:city a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string .

ex:zipCode a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string .

ex:marketValue a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:decimal .

# Attributes for IndustryDomain
ex:industryName a rdf:Property ;
    rdfs:domain ex:IndustryDomain ;
    rdfs:range xsd:string .

# Skill
ex:skillName a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:string .

ex:skillLevel a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:string .

ex:skillScore a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:decimal .

# Benefit
ex:benefitType a rdf:Property ;
    rdfs:domain ex:Benefit ;
    rdfs:range xsd:string .

ex:economicalValue a rdf:Property ;
    rdfs:domain ex:Benefit ;
    rdfs:range xsd:decimal .
```
    
b. Express which classes are equivalent and which ones are disjoint.

In RDFS and OWL the semantics expresses that:

- Open World Assumption The absence of a triple in a graph does not imply that the corresponding statement does not hold
    
- No Unique Name Assumption: differently named individuals can denote the same thing

Given that, we can for example express that "Job" and "JobOffer" are indeed the same concept in our domain:
```
ex:JobOffer a owl:Class ;
owl:equivalentClass ex:Job .
```

These are instead all disjoint specifications:
```
ex:Job owl:disjointWith ex:Company, ex:IndustryDomain, ex:Skill, ex:Benefit .
ex:Company owl:disjointWith ex:IndustryDomain, ex:Skill, ex:Benefit .
ex:IndustryDomain owl:disjointWith ex:Skill, ex:Benefit .
ex:Skill owl:disjointWith ex:Benefit .
```

c. Specify (or add) at least an inverse property.

Let's repeat once again all the (Subject to Object) properties in order to define their possible inverse relationships:
```
ex:belongsTo a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range ex:Company ;
    owl:inverseOf ex:offersJob .

ex:requires a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range ex:Skill ;
    owl:inverseOf ex:isRequired .

ex:offers a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range ex:Benefit ;
    owl:inverseOf ex:isOffered .

ex:operatesIn a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range ex:IndustryDomain ;
    owl:inverseOf ex:hasCompany .
```
    
d. For all the modeled properties, specify whether they are functional (or inverse functional).

- The only Class Property to be functional is belongsTo, because if a Job belongs to CompanyA and CompanyB that two Companies are indeed the same Company.
- There are no Inverse Functional Class Properties
- All the Literal Properties are functional (there aren't any entities that can have multiple literal values, e.g a single Job Offer can't have two different expire dates, therefore if a Job has two expire_dates they're indeed the same expire_date.
- Only the PRIMARY KEY literals are also inverse functional (e.g if two Companies have the same name, the two Companies are indeed the same Company.

```
# Subject to Object Properties
ex:belongsTo a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range ex:Company ;
    a owl:FunctionalProperty .

# Subject to Literal Properties - Job
ex:jobTitle a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty, owl:InverseFunctionalProperty .

ex:jobType a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty .

ex:expireDate a rdf:Property ;
    rdfs:domain ex:Job ;
    rdfs:range xsd:date ;
    a owl:FunctionalProperty .

# Subject to Literal Properties - Company
ex:companyName a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty, owl:InverseFunctionalProperty .

ex:country a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty .

ex:city a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty .

ex:zipCode a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty .

ex:marketValue a rdf:Property ;
    rdfs:domain ex:Company ;
    rdfs:range xsd:decimal ;
    a owl:FunctionalProperty .

# Attributes for IndustryDomain
ex:industryName a rdf:Property ;
    rdfs:domain ex:IndustryDomain ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty, owl:InverseFunctionalProperty .

# Subject to Literal Properties - Skill
ex:skillName a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty, owl:InverseFunctionalProperty .

ex:skillLevel a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty .

ex:skillScore a rdf:Property ;
    rdfs:domain ex:Skill ;
    rdfs:range xsd:decimal ;
    a owl:FunctionalProperty .

# Subject to Literal Properties - Benefit
ex:benefitType a rdf:Property ;
    rdfs:domain ex:Benefit ;
    rdfs:range xsd:string ;
    a owl:FunctionalProperty, owl:InverseFunctionalProperty .

ex:economicalValue a rdf:Property ;
    rdfs:domain ex:Benefit ;
    rdfs:range xsd:decimal ;
    a owl:FunctionalProperty .
```

## (15) Model RDF instances

```
ex:SwEng a ex:Job ;
    ex:jobTitle "Software Engineer" ;
    ex:jobType "Full-time" ;
    ex:expireDate "2025-02-15"^^xsd:date ;
    ex:belongsTo ex:GueCorp ;
    ex:requires ex:Pitch, ex:SQL ;
    ex:offers ex:HI, ex:TaxBenefit .

ex:GueCorp a ex:Company ;
    ex:companyName "GuerriniCorp" ;
    ex:country "Italy" ;
    ex:city "Genoa" ;
    ex:zipCode "16165" ;
    ex:marketValue "80000000.00"^^xsd:decimal ;
    ex:operatesIn ex:Creativity .

ex:Creativity a ex:IndustryDomain ;
    ex:industryName "Creativity" .

ex:Pitch a ex:Skill ;
    ex:skillName "Elevator Pitch" ;
    ex:skillLevel "Intermediate" ;
    ex:skillScore "85.0"^^xsd:decimal .

ex:SQL a ex:Skill ;
    ex:skillName "SQL" ;
    ex:skillLevel "Advanced" ;
    ex:skillScore "90.0"^^xsd:decimal .

ex:HI a ex:Benefit ;
    ex:benefitType "Health Insurance" ;
    ex:economicalValue "5000.00"^^xsd:decimal .

ex:TaxBenefit a ex:Benefit ;
    ex:benefitType "401(k)" ;
    ex:economicalValue "3000.00"^^xsd:decimal .
```

Let's now identify which instances are the same and which are different among each other.

For example, if I were to add a "SoftwareEng" node, for the same reason of the "Paris-Parigi" example discussed in class, it has to be referred as being the same as SwEng!

```
ex:SoftwareEng a ex:Job ;
    ex:jobTitle "Software Engineer" ;
    ex:jobType "Full-time" ;
    ex:expireDate "2025-02-15"^^xsd:date ;
    ex:belongsTo ex:GueCorp ;
    ex:requires ex:Pitch, ex:SQL ;
    ex:offers ex:HI, ex:TaxBenefit .

ex:SwEng owl:sameAs ex:SoftwareEng .
```

Regarding the "differentFrom" constraint, since we have already defined that our classes are distinct from each other using the "disjointWith" constraint, this information is implicitly inferred. Therefore, we only need to specify that nodes within the same class are distinct from one another:

```
ex:Pitch owl:differentFrom ex:SQL .
ex:HI owl:differentFrom ex:TaxBenefit .
```

## (16) SPARQL queries

For the first query we refer to our Query #1 and we decide to translate it in SPARQL paying attention on how to filter the date attribute:
```
SELECT ?jobTitle
WHERE {
  ?job a ex:Job ;
       ex:jobType "Full-time" ;
       ex:expireDate ?expireDate ;
       ex:jobTitle ?jobTitle .
  FILTER (xsd:date(?expireDate) <= xsd:date(NOW()) + "P30D"^^xsd:duration)
}
```

For the second query we refer to our Query #4 so that we can introduce the DISTINCT construct (differently from the previous query we're returning non-unique values):
```
SELECT DISTINCT ?jobType
WHERE {
  ?company ex:operatesIn ?industry ;
           ex:belongsTo ?job .
  ?industry a ex:IndustryDomain ;
            ex:industryName "Technology" .
  ?job ex:jobType ?jobType .
}
```

For the third query we want to use (as the assignment requires) the CONSTRUCT keyword:

Construct a graph with all jobs that require skills with a level of Beginner.
```
CONSTRUCT {
  ?job a ex:Job ;
       ex:requires ?skill .
}
WHERE {
  ?job a ex:Job ;
       ex:requires ?skill .
  ?skill ex:skillLevel "Beginner" .
}
```
We note that this redundancy inside both CONSTRUCT and WHERE constructs is actually a feature of SPARQL, which allows the user to specify different conditions with respect to the final graph that will be indeed produced!

For a fourth query we try to specify a query that involves both OPTIONAL and UNION clauses:

Retrieve jobs with their associated skills, if available, where jobs either belong to companies in Italy OR operate in the Technology industry.
```
SELECT ?jobTitle ?skillName
WHERE {
  {
    {
      ?job a ex:Job ;
           ex:belongsTo ?company ;
           ex:jobTitle ?jobTitle .
      ?company ex:country "Italy" .
    }
    UNION
    {
      ?job a ex:Job ;
           ex:belongsTo ?company ;
           ex:jobTitle ?jobTitle .
      ?company ex:operatesIn ?industry .
      ?industry ex:industryName "Technology" .
    }
  }
  
  OPTIONAL {
    ?job ex:requires ?skill .
    ?skill ex:skillName ?skillName .
  }
}
```

We end with a fifth query to also cover the "ASK" construct:

Check if any job requires a skill with a level of Beginner.

```
ASK {
  ?job a ex:Job ;
       ex:requires ?skill .
  ?skill ex:skillLevel "Beginner" .
}
```

## (17) Let's try 14-16 content in RDF playground!

We "played" into the "play"ground and verified the correctness of all classes, properties, constraints and SPARQL queries.
For obtaining a more meaningful and enriched graph we also added some other instances. You can find the whole dataset at this link: (https://raw.githubusercontent.com/enriicola/Linked-adm-In/refs/heads/main/SPARQL-data-and-queries.ttl)

![SPARQL-graph](https://github.com/user-attachments/assets/0d5a7ed7-f37c-46df-b53c-b029cbcd0164)

Below, for reference, there are results of the queries for the above dataset:

```
# --- 1 ---
--------------------
| jobTitle         |
====================
| "Data Scientist" |
--------------------

# --- 2 ---
-----------
| jobType |
===========
-----------

# --- 3 ---
(graph
  (triple ex:InternDev ex:requires ex:HTML)
  (triple ex:InternDev ex:requires ex:CSS)
  (triple ex:InternDev rdf:type ex:Job)
)

# --- 4 ---
------------------------------------------
| jobTitle            | skillName        |
==========================================
| "Developer Intern"  | "CSS"            |
| "Developer Intern"  | "HTML"           |
| "Software Engineer" | "SQL"            |
| "Software Engineer" | "Elevator Pitch" |
| "Software Engineer" | "Elevator Pitch" |
| "Software Engineer" | "SQL"            |
| "Developer Intern"  | "CSS"            |
| "Developer Intern"  | "HTML"           |
| "Data Scientist"    | "SQL"            |
| "Data Scientist"    | "Python"         |
------------------------------------------

# --- 5 ---
yes
```

# Presentation Link :)
https://docs.google.com/presentation/d/10JpM2nPsat2lPP40ubgr755MYcMWq2DNsRzwk-B55IA/edit?usp=sharing 
