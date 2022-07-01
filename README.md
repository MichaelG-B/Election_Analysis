# Election Analysis with Python VS Code

## Overview of Election Audit

- Link to project file
- https://github.com/MichaelG-B/Stock-analysis/blob/bd67d3f1078e1bb428f11d2a49fb341d00c3b4ed/VBA_Challenge.xlsm

### Purpose

- The purpose of this project was to assist a colorado board of elections official carry out an election audit of the tabulated results for a US congressional precinct in Colorado. The Audit focused on finding the total number of votes cast, the total number of votes for each candidate, the percentage of votes for each candidate and the winner of the election based on popular vote. The audit also focused on the results in relation to the counties within the US congressional precinct including the voter turnout for each county, the percentage of votes from each county out of the total count, and the county with the highest turnout. Collectively this analysis allowed us to certify the results of the election efficiently and accurately.

### Analysis
## The Data

-The Election Data we analyzed provided us with a Ballot ID, the County of the Ballot, and the Candidate name voted for in the Ballot.

## Election Audit Results

- Here is our Python code we developed to conduct our analysis of the Election Data.

```
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge Solution."""

# Add our dependencies.
import csv
import os

# Add a variable to load a file from a path.
file_to_load = os.path.join("Resources", "election_results.csv")
# Add a variable to save the file to a path.
file_to_save = os.path.join("analysis", "election_analysis.txt")

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_names =[]
county_votes = {}


# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0
winning_county = 0
winning_county_percentage = 0

# 2: Track the largest county and county voter turnout.
county_largest_turnout_name = ""
county_largest_turnout_votes = 0


# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]


        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_names:

            # 4b: Add the existing county to the list of counties.
            county_names.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1



# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    for county_name in county_votes:

        # 6b: Retrieve the county vote count.
        county_vote_count = county_votes.get(county_name)

        # 6c: Calculate the percentage of votes for the county.
        county_vote_percentage = float(county_vote_count) / float(total_votes) * 100

         # 6d: Print the county results to the terminal.
        county_results = (f"{county_name}: {county_vote_percentage:.1f}% {county_vote_count:,})\n")
        print(county_results, end="")

         # 6e: Save the county votes to a text file.
        txt_file.write(county_results)
         
         # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote_count > winning_county) and (county_vote_percentage > winning_county_percentage):
            winning_county = county_vote_count
            winning_county_name = county_name
            winning_county_percentage = county_vote_percentage


    # 7: Print the county with the largest turnout to the terminal.
    winning_county_summary = (
        f"\n-------------------------\n"
        f"Largest County Turnout: {winning_county_name}\n"
        f"Winning County Vote Count: {winning_county:,}\n"
        f"winning County Percentage: {winning_county_percentage:.1f}%\n"
        f"-------------------------\n\n")
    print(winning_county_summary)


    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_summary)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)

```

- As well as screen shots of our Election Audit results.
1. Total number of votes cast in the congressional election
![PNg 1.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/1..png)
2. Number and percentage of total votes for each county in the precinct
![PNG 2.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/2..png)
3. County with the largest number of votes
![PNG 3.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/3..png)
4. Number and percentage of total votes for each candidate
![PNG 4.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/4..png)
5. Election winner as well as their vote count and percentage of total votes
![PNG 5.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/5..png)
6. Results in our election_results.txt file
![Txt file PNG.Analysis](https://github.com/MichaelG-B/Election_Analysis/blob/8c8a0219ac7a485d8812a5758724678f45d1340c/election_analysis.txt)

## Election Audit Summary

- This election audit script could be used by the election commission for future elections via simply upload and applying this script to another .csv file. If the .csv file in question has the same data as mentioned above, a Ballot ID, the County of the Ballot, and the Candidate name voted for in the Ballot, then it is essentialy a repitition of the same analysis we just completed. 
- Two adjustments to the scipt that could be useful however would be to include the amount of votes in the winning county + the percentage of votes in the winning county, this could provide more information as to what demograpahic voted for the majority of the winning candidate. Another adjustment might be to include party affiliation in the .csv file in order for political parties to analyze the date and prepare a decent strategy for the next election cylce based on the previous results.

