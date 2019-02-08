# Bibtex-to-Neo4j

var bibtexParse = require('bibtex-parse-js');
var fs = require('fs');

function convertBibtex(inFile, outFile) {
    fs.readFile(inFile, 'utf8', function (err, data) {
        if (err) throw err;
        data = bibtexParse.toJSON(data);
        const asString = JSON.stringify(data);
        fs.writeFile(outFile, asString, function(err) {
            if (err) throw err;
        });
        convertJsonToGraphqlMutation(data);
    });
}

function convertJsonToGraphqlMutation(articles) {
    // const citedBy = []; use later 
    articles.forEach(e => {
        const entryTags = e.entryTags;

        if(entryTags) {
            const cites = e.entryTags["Cited-References"];
            const splitCitations = cites.split(",");

            // 1. get the cited article authorName as the word before the first ','
            // 2. go through all articles to find the article with an .entryTags.Author === authorName
            console.log("splitCitations: ", splitCitations);
        }
    })    
}

convertBibtex('./50_TCE_Example_all.bib', './50_TCE_Example_converted.json');
