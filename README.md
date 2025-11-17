# kautilya-OA

Overview
This project implements a narrative generation system for an 84 MB news dataset as part of Task 2 of the Kautilya ML Challenge.
The goal is to take any user provided topic from the command line and automatically generate a structured narrative that includes a summary, a chronological timeline, thematic clusters, and a simple narrative graph describing how the articles relate to each other.

What the System Does
The system loads a large JSON or JSONL news dataset, filters articles by source rating to ensure credibility, embeds all relevant articles, identifies those most connected to the user topic, and produces a narrative based on semantic similarity.
The final output is a JSON object containing four sections: narrative summary, timeline, clusters, and graph.

How It Works

Dataset Loading
The script loads both JSON arrays and JSON lines formats.
It supports datasets where articles may be nested under fields such as articles, data, docs or other list based keys.
Only valid article objects are kept for processing.

Filtering by Source Rating
The system keeps only articles with a source rating greater than a threshold value.
This ensures the narrative is based on higher quality information.
The default threshold is 8 point zero.

Embedding and Semantic Retrieval
Each article is converted into a text representation made from its title, description, and content.
These are embedded using the all-MiniLM-L6-v2 sentence transformer model.
A user provided topic is embedded the same way.
Cosine similarity is used to score each article relative to the topic.
The system selects the most relevant articles based on similarity and a maximum article limit.

Narrative Summary
A high level summary is generated using the top article titles.
It explains what the narrative covers and highlights the major storylines related to the topic.

Timeline Construction
The system extracts all available dates, sorts the articles chronologically, and produces a list containing
date
headline
url
why the article matters
This forms a clear chronological storyline for the topic.

Clustering
KMeans is applied to the article embeddings to group them into thematic clusters.
Each cluster is given a readable label generated from common words in the article titles.

Narrative Graph
A simple relationship graph is created where each article is a node.
Edges are added based on chronological order and shared context.
This represents how different parts of the story build on one another.

How to Run the System

Run the script from the command line by specifying the dataset path and the topic
Example
python narrative_builder.py --data_path "/content/news.jsonl" --topic "AI regulation"

You can run any topic such as
python narrative_builder.py --topic "Jubilee Hills elections"
python narrative_builder.py --topic "Israel Iran conflict"
python narrative_builder.py --topic "AI regulation"

Required Arguments
data_path
Full path to the news dataset
topic
The topic for which the narrative should be generated

Optional Arguments
min_source_rating
Default is 8 point zero
max_articles
Default is 100
n_clusters
Default is 4

Output
The script prints a single JSON object containing
narrative_summary
timeline
clusters
graph

Why This Project Matters
Large news datasets can be overwhelming to navigate manually.
This system automatically synthesizes large volumes of text into a structured narrative using embeddings, semantic similarity, clustering, and lightweight graph construction.
It transforms unstructured data into a coherent storyline while remaining efficient enough to run on standard hardware.

Summary
This project delivers a complete workflow for topic driven narrative generation from large news datasets.
It covers data loading, filtering, embedding, article selection, timeline modeling, clustering, and graph construction.
The approach demonstrates practical experience in semantic retrieval, unsupervised learning, and narrative modeling, all packaged into a simple command line interface suitable for real world applications.
