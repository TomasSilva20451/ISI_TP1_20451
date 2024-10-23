# ETL Project: Football Data Integration

## Overview
This project involves the integration and processing of football-related data using **Pentaho Data Integration (PDI/Kettle)**. The project extracts, transforms, and loads (ETL) data from multiple tables in a PostgreSQL database to produce clean, structured outputs in various formats (TXT, XML, JSON). The focus is on integrating data related to **jogos, estadios, jogadores, and eventos**, ensuring quality and readiness for further analysis.

## Table of Contents
- [Project Overview](#overview)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Transformations](#transformations)
  - [Transformation 1: Cleaning and Enriching Stadium Data](#transformation-1-cleaning-and-enriching-stadium-data)
  - [Transformation 2: Data Normalization and Validation](#transformation-2-data-normalization-and-validation)
  - [Transformation 3: Data Sorting and Filtering](#transformation-3-data-sorting-and-filtering)
  - [Transformation 4: Player Data Processing](#transformation-4-player-data-processing)
  - [Transformation 5: Match Data Export to JSON](#transformation-5-match-data-export-to-json)
- [Job Execution](#job-execution)
  - [Error Handling](#error-handling)
  - [Notifications and Logging](#notifications-and-logging)
- [Setup Instructions](#setup-instructions)
- [Future Improvements](#future-improvements)
- [Contributors](#contributors)

## Project Structure
/ETL-Project
│
├── datainit/
│   ├── Transformation 1.ktr
│   ├── Transformation 2.ktr
│   ├── Transformation 3.ktr
│   ├── Transformation 4.ktr
│   └── Transformation 5.ktr
│   └── Executar_Todas_Transformacoes.kjb
│   └── logs/
│       ├── Transformacao1.log
│       ├── Transformacao2.log
│       ├── Transformacao3.log
│       ├── Transformacao4.log
│       └── Transformacao5.log
│
├── data/
│   └── input/
│   └── output/
│       ├── .xml
│       ├── .js
│       └── .txt
│
├── doc/
│   └── tp01_20451_doc.pdf/
│
├── doc/
│
├── README.md
└── setup/
    └── database_backup.sql


## Technologies Used
- **Pentaho Data Integration (PDI/Kettle)**: Used for designing ETL transformations and jobs.
- **PostgreSQL**: Database management system to store and manage the football data.
- **pgAdmin**: For managing the PostgreSQL database and performing backup/restore operations.
- **MacOS Sequoia**: Development environment for the project.

## Transformations

### Transformation 1: Cleaning and Enriching Stadium Data
- **Objective**: Extracts data from the stadiums table for venues with a capacity greater than 10,000 and enriches it with additional information about the country.
- **Steps**:
  1. Read stadium data where capacity > 10,000.
  2. Filter out irrelevant records.
  3. Use regex to clean stadium names by removing numbers.
  4. Export to a text file.
- **Output**: stadiums_cleaned.txt

### Transformation 2: Data Normalization and Validation
- **Objective**: Clean and normalize player data, ensuring consistency and validity.
- **Steps**:
  1. Remove extra spaces and internal spaces in player names.
  2. Convert to uppercase and replace special characters.
  3. Validate capacity values using a value mapper.
  4. Output to XML format.
- **Output**: normalized_players.xml

### Transformation 3: Data Sorting and Filtering
- **Objective**: Sort match and stadium data, filter out invalid entries, and log errors.
- **Steps**:
  1. Sort rows by match date and stadium capacity.
  2. Filter out invalid data and write to a separate error log.
  3. Add date and time to the output file names.
- **Output**: match_data_sorted.txt, stadiums_cleaned.txt, error_logs/

### Transformation 4: Player Data Processing
- **Objective**: Clean and structure player data for better usability.
- **Steps**:
  1. Validate data fields like Nome and DataNasc.
  2. Sort by Época.
  3. Export to TXT with a footer summary.
- **Output**: players_cleaned.txt

### Transformation 5: Match Data Export to JSON
- **Objective**: Export match data from the database to a structured JSON format.
- **Steps**:
  1. Extract match details, including teams, dates, and scores.
  2. Adjust column names for better readability.
  3. Write to a JSON file.
- **Output**: match_data.json

## Job Execution
### Structure
The job, execute_all_transformations.kjb, follows a linear execution path:
Start → Execute Transformation 1 → Execute Transformation 2 → Execute Transformation 3 → Execute Transformation 4 → Execute Transformation 5 → Success

### Error Handling
- **Current Setup**: The job stops if a transformation fails.
- **Suggestions for Improvement**:
  - Add conditional logic to log errors and continue to the next transformation.
  - Configure transformations to redirect error outputs to a specified log file.

### Notifications and Logging
- Add optional email notifications for job completion status.
- Consider using a consolidated log file for better debugging and process monitoring.

## Setup Instructions
1. **Database Setup**:
   - Restore the database_backup.sql provided in the setup/ directory.
   - Use pgAdmin or psql to execute the backup file:
     
bash
     psql -U username -d database_name -f database_backup.sql

2. **Pentaho Data Integration**:
   - Install PDI (Pentaho Data Integration) and open the Spoon interface.
   - Import all transformations and jobs from the transformations/ and jobs/ directories.
3. **Running the Job**:
   - Open the job file execute_all_transformations.kjb and execute it to process all transformations sequentially.

## Future Improvements
- **Enhanced Error Handling**: Implement better error management to allow partial success and detailed logging.
- **Parallel Execution**: Modify the job to execute transformations in parallel where dependencies allow.
- **Real-Time Data Integration**: Explore options to automate the ETL process with periodic triggers and real-time data integration.
- **Advanced Notifications**: Implement email or webhook notifications for status updates.

## Contributors
- **Tomás Silva** - Developer & Project Manager

## References
- **Pentaho Documentation**: [Pentaho Data Integration](https://help.pentaho.com/)
- **PostgreSQL Documentation**: [PostgreSQL](https://www.postgresql.org/docs/)
