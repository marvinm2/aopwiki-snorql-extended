# AOP-Wiki SPARQL Interface

A modern web-based SPARQL query interface for [AOP-Wiki](https://aopwiki.org/) (Adverse Outcome Pathway Wiki). This application provides an intuitive interface for querying the AOP-Wiki RDF data via SPARQL.

**Live Endpoint**: https://aopwiki.rdf.bigcat-bioinformatics.org/sparql/

## Features

- **CodeMirror Editor**: SPARQL syntax highlighting, line numbering, bracket matching, and fullscreen mode
- **Query Examples**: Fetches SPARQL queries (.rq files) from GitHub repositories on the fly
- **Export Formats**: CSV, JSON, and XML export options
- **Permalinks**: Generate shareable URLs for your queries
- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Prefix Management**: Shows available namespace prefixes from the endpoint

## Quick Start

### Docker Deployment (Recommended)

```bash
# Build and run
docker build -t aopwiki-snorql .
docker run -p 8088:80 aopwiki-snorql

# Access at http://localhost:8088
```

### Local Development

Since this is a pure frontend application, serve it with any web server:

```bash
# Python
python3 -m http.server 8000

# Node.js
npx serve .

# Access at http://localhost:8000
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `SNORQL_ENDPOINT` | SPARQL endpoint URL | `https://aopwiki.rdf.bigcat-bioinformatics.org/sparql/` |
| `SNORQL_EXAMPLES_REPO` | GitHub repository for query examples | `https://github.com/marvinm2/AOPWikiSNORQL` |
| `SNORQL_TITLE` | Application title | AOP-Wiki SPARQL |
| `DEFAULT_GRAPH` | Default RDF graph URI | (empty) |

### Manual Configuration

Edit `assets/js/snorql.js` to modify:
- `_endpoint` - SPARQL endpoint URL
- `_examples_repo` - GitHub repository for examples
- `_defaultGraph` - Default graph URI

Namespace prefixes are defined in `assets/js/namespaces.js`.

## Data Pipeline

### Weekly Updates

AOP-Wiki RDF data is regenerated weekly (Saturdays at 08:00 UTC) from:
- AOP-Wiki XML exports
- HGNC gene data
- BridgeDb identifier mapping services

### Data Files Served

| File | Description |
|------|-------------|
| `AOPWikiRDF.ttl` | Main dataset (AOPs, Key Events, Key Event Relationships, Chemical Stressors) |
| `AOPWikiRDF-Genes.ttl` | Gene mapping extensions with HGNC symbols and database identifiers |
| `AOPWikiRDF-Void.ttl` | VoID metadata describing the datasets |
| `ServiceDescription.ttl` | SPARQL service description with endpoint capabilities |

Data generation is handled by the [AOPWikiRDF repository](https://github.com/marvinm2/AOPWikiRDF).

## Deployment

### Docker Compose (Full Stack)

```bash
docker-compose up
```

Configure the placeholders in `docker-compose.yml`:
- `VIRTUOSO_CONTAINER_NAME` - Container name for Virtuoso
- `VIRTUOSO_DB_PERSISTENT_DIR` - Database persistence directory
- `CONDUCTOR_PASSWORD` - Virtuoso admin password
- `SNORQL_CONTAINER_NAME` - Container name for SNORQL
- `SNORQL_WEB_FILES_DIR` - Web files directory

### Data Loading

Use `load.sh` to load RDF data into Virtuoso:

```bash
./load.sh /path/to/logfile virtuoso_password
```

## Namespace Prefixes

The interface includes extensive biological and chemical ontology prefixes:

**AOP-specific**
- `aop:` - AOP identifiers
- `aop.events:` - Key Event identifiers
- `aop.relationships:` - Key Event Relationship identifiers
- `aop.stressor:` - Stressor identifiers
- `aopo:` - AOP Ontology

**Chemical**
- `chebi:`, `cas:`, `inchikey:`, `chembl.compound:`, `pubchem.compound:`

**Biological**
- `go:`, `hgnc:`, `ncbigene:`, `uniprot:`, `ensembl:`

**Ontologies**
- `pato:`, `ncbitaxon:`, `cl:`, `uberon:`, `hp:`, `mp:`

See `assets/js/namespaces.js` for the complete list.

## Related Resources

- [AOP-Wiki](https://aopwiki.org/) - Source database
- [AOPWikiRDF](https://github.com/marvinm2/AOPWikiRDF) - RDF data generation repository
- [AOPWikiSNORQL](https://github.com/marvinm2/AOPWikiSNORQL) - SPARQL query examples
- [SNORQL Extended](https://github.com/ammar257ammar/snorql-extended) - Upstream SNORQL project

## Maintainers

- **Lead maintainer:** Marvin Martens — Department of Translational Genomics, Maastricht University — [ORCID 0000-0003-2230-0840](https://orcid.org/0000-0003-2230-0840)
- **Backup maintainer:** Egon Willighagen — Department of Translational Genomics, Maastricht University — [ORCID 0000-0001-7542-0286](https://orcid.org/0000-0001-7542-0286)

For questions, bug reports, and feature requests please open a [GitHub Issue](https://github.com/marvinm2/aopwiki-snorql-extended/issues).

## License

- **This UI codebase**: GNU General Public License v3.0 (GPL-3.0) — see [`LICENSE`](LICENSE). The codebase derives from [SNORQL Extended](https://github.com/ammar257ammar/snorql-extended), which builds on the original [SNORQL](https://github.com/kurtjx/SNORQL) concept.
- **Served AOP-Wiki RDF dataset**: Creative Commons Attribution-ShareAlike 4.0 International (CC-BY-SA 4.0) — see the [AOPWikiRDF data licence](https://github.com/marvinm2/AOPWikiRDF/blob/master/data/LICENSE-DATA). Matches the upstream [AOP-Wiki](https://aopwiki.org/) content licence.
