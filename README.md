# Visualizing a Book with NLP & Interactive Web-based Visualizations

This repository contains the source code for the implementation of an interactive visual analytics tool which can be used to analyze the content of a novel.

The app was created within the project module ***VIS-Proj-M: Visualizing a Book*** (Group A) at University of Bamberg. It was an interdisciplinary master’s project that combined Natural Language Processing (NLP) with Data Visualization to explore unstructured literary texts. We analyzed _Anne of Green Gables_ by L. M. Montgomery using modern NLP techniques, and visualized character interactions, sentiment trajectories, and topic structures in the book using D3.js and Vue.js. 

> A working version prototype can be accessed at https://bookvis-anne-of-green-gables.netlify.app


## Project Highlights

- **Text Processing:** Cleaned and preprocessed raw text, incl. tokenization, lemmatization and N-gram processing.

- **Named Entity Recognition (NER)**: Extracted character mentions from a chapter or conversation, useful for mapping character interactions.

- **Semantic Topic Analysis:** Generated and visualized topics and their semantic distances using Latent Dirichlet Allocation, TF-IDF, and dimensionality reduction.

- **Interactive Visualization Dashboard:** Designed and developed a dynamic web dashboard featuring word clouds, topic maps, character networks, and sentiment-over-time charts. Some of these visualizations can interact with each other upon clicking on certain visual components.


## Visual Analysis Features

- **Book Overview:** Key statistics, word clouds, and top terms.
- **Character Analysis:** Character Network & Word Clouds, Character Occurences & Sentiment Evolution charts 
- **Topic Analysis:** Chapter-level Word Clouds, Intertopic Distance Map and Top Terms per Topic Map based on LDA-based topic modeling.


## Tech Stack

- **NLP Analysis:** `spaCy`, `gensim`, `SciPy`, `NetworkX`
- **Web Visualizations:** `D3.js`, `Vue.js`, `HTML/CSS`, `JavaScript`


## Instructions for Project Setup

### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
