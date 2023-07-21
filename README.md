<!DOCTYPE html>
<html>
<head>

</head>
<body>
<h1>Apache-Spark-BigData-DPHQueryProcessorRanker</h1>
<p>This project implements a DPH (Divergence from Randomness model) Query Processor and Document Ranker using Apache Spark and Big Data technologies. The primary purpose of this application is to retrieve and rank the top 10 documents based on DPH score relevance ranking of user-defined queries.</p>

<h2>Program Logic</h2>
<p>
    The application begins by broadcasting the user defined queries for processing. It also applies a filter function to eliminate null values on id, title, contents, and subtype columns of news articles. Additionally, accumulators are initialized to compute parameters for the DPH scoring model. Following the initial setup, the application applies a series of transformations and actions to process the data, compute the DPH scores, and finally return the top 10 results per query, ensuring that overly similar documents are filtered out. The implementation involves three core custom functions to execute these tasks.
</p>

<h2>Custom Functions</h2>
<p>
    The three custom functions are as follows:
    <ul>
        <li>
            <strong>NewsPreprocessor:</strong> A map function that processes and filters the news articles dataset. It performs stemming and tokenization on the articles and contents. In the process, it also computes the query term frequency in the entire corpus using accumulators. This data is critical for the DPH scoring model.
        </li>
        <li>
            <strong>NewsArticleDPHProcessor:</strong> A flatmap function that computes the average DPH Score for a given query using the processed news articles from the NewsPreprocessor function, document length, query term frequency, and total corpus document accumulators. This function essentially handles the computation of DPH scores.
        </li>
        <li>
            <strong>QueryGroupsRanking:</strong> A flatmapgroups function that groups the NewsArticleDPHScore objects by query and computes the top 10 DPH Score ranked documents per query. It sorts the documents based on DPH scores, filters out the ones with high similarity score, and generates the final ranked document structure.
        </li>
    </ul>
</p>

<h2>Efficiency & Scalability</h2>
<p>
    The primary objective during the development was to ensure the application is efficient and can scale with increasing data volumes. Hence, all computational tasks were executed within Spark transformations to increase parallelism and reduce computational bottlenecks. Broadcast variables were used to reduce redundancy, and parameters were pre-computed wherever possible. Additional measures such as early null-checks, avoiding for-loops and efficient usage of accumulators further contributed to the application's efficiency.
</p>

<h2>Challenges & Resolutions</h2>
<p>
    Some of the significant challenges faced during development included handling large datasets, addressing null pointer exceptions, handling data dependencies among transformations, and choosing optimal transformation functions. These were resolved by using a hashmap based accumulator for efficient computation, using the filter method for handling null-values, passing the NewsArticle object through transformations to retain the original object, and careful selection of transformations to ensure data is correctly passed between transformations.
</p>

<h2>Execution Time</h2>
<p>
    The execution time for the entire program on a large news articles dataset is within 1 minute 3 seconds. This is a testament to the efficiency of the program logic and the optimizations incorporated into the code.
</p>

<h2>Usage</h2>
<p>
    To use this application, compile and package it using maven or sbt, and then submit it to a Spark cluster using the spark-submit script. Provide the necessary arguments such as the path to the input data and the query terms. Detailed instructions for usage and examples can be found in the project's documentation.
</p>


</body>
</html>
