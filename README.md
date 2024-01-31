[TheyWorkForYou]: https://www.theyworkforyou.com/
# Analysing UK's Parliamentary Mentions of Hong Kong
## Overview
The UK has a long historical relationship with Hong Kong. Although Hong Kong ceased to be a British colony in 1997, the Sino-British Joint Declaration of 1984 binds the UK Government with an obligation oversee the implementation of the treaty's principles. While the Foreign, Commonwealth & Development Office's [six-monthly reports on Hong Kong](https://www.gov.uk/government/collections/) published since July 1997 have constituted a core part of this effort, there have also been consistent parliamentary discussions on Hong Kong in both the UK Parliament and regional assemblies, with a notable increase in mentions since the 2019 Anti-Extradition Bill Movement and the National Security Law's introduction.

This project analyses the trend of Hong Kong mentions in the UK Parliament over time, focusing on the varied attention and opinions of different parties and politicians regarding Hong Kong. Key outputs include a dataset of parliamentary mentions since 1919 and visualisations of each party's annual mention frequency from 2002 to 2023, adjusted for the number of seats held. The next phase involves analysing the content of these speeches for deeper insights.

## Speech and person dataset
The dataset, 'all_speeches_and_person.csv,' encompasses 16,436 records of parliamentary speeches mentioning 'Hong Kong', sourced from [TheyWorkForYou]'s extensive archive. This archive surpasses the limitations of the UK Parliament's [Hansard search](https://hansard.parliament.uk/search) and [API](https://developer.parliament.uk/), offering comprehensive data including debates, written answers, and ministerial statements across various periods and parliaments. The dataset's constraints align with the coverage of [TheyWorkForYou], detailed [here](https://www.theyworkforyou.com/help/).

Each record in the dataset contains 21 fields. Here's a specification of what each field means:
| Column                 | Description                                                                                                                                                                       |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gid                    | Unique ID for each debate, which typically comprises multiple speeches.                                                                                                           |
| hdate                  | Date of the speech.                                                                                                                                                               |
| parent_body            | Title of the debate.                                                                                                                                                              |
| file_name              | JSON file from which the debate data was retrieved.                                                                                                                               |
| html_file_name         | HTML file from which the speech was extracted.                                                                                                                                    |
| debate_type            | Category of the debate, including 'Commons debates', 'Written Answers', 'Scottish Parliament debates', 'Lords debates', 'Scottish Parliament written answers', 'Westminster Hall debates', 'Northern Ireland Assembly debates', 'Written Ministerial Statements', 'Welsh Parliament record', 'Questions to the Mayor of London', and 'Public Bill Committees'. |
| written_type           | Indicates if the speech is a question or an answer in written debates.                                                                                                            |
| speech_body            | The actual content of the speech.                                                                                                                                                 |
| full_url               | Link to the debate's page on [TheyWorkForYou].                                                                                                                                    |
| relevant_speeches      | Count of speeches mentioning 'Hong Kong' in the same debate.                                                                                                                      |
| speaker_id and person_id | IDs for uniquely identifying MPs, as used by [Public Whip](https://www.publicwhip.org.uk/) and [TheyWorkForYou].                                                                   |
| speaker_name           | Current name of the speaker, noting that name changes are common among Lords.                                                                                                     |
| membership_id          | ID for identifying each person's office, as per [people.json](https://parser.theyworkforyou.com/members.html).                                                                    |
| membership_start_date  | Start date of the speaker's office term.                                                                                                                                          |
| membership_end_date    | End date of the speaker's office term.                                                                                                                                            |
| post_id                | ID for identifying each constituency; not applicable to Lords.                                                                                                                    |
| post_name              | Name of the speaker's post.                                                                                                                                                       |
| post_area_name         | Area of the speaker's post.                                                                                                                                                       |
| party_id               | ID for identifying political parties, as per [people.json](https://parser.theyworkforyou.com/members.html).                                                                       |
| party_name             | Name of the speaker's party.                                                                                                                                                      |

### Parsing getHansard's results
The process of creating the dataset began with querying 'Hong Kong' in [TheyWorkForYou]'s [getHansard](https://www.theyworkforyou.com/api/docs/getHansard), yielding 7,683 results. Through '1_extract_hansard_hong_kong_jsons.ipynb', the results were parsed into a table, 'hansard_hong_kong.csv', and stored in the 'intermediate_outputs' folder. The 'file_name' column in the final dataset corresponds to the JSON result's page number.

### Extracting collapsed speeches
[getHansard](https://www.theyworkforyou.com/api/docs/getHansard)'s results often include collapsed speeches from a single debate. To capture these, '2_extract_collapsed_speeches.ipynb' retrieves and extracts them from the corresponding HTML pages on [TheyWorkForYou], saving the results in 'scrape_results.csv'. The 'html_file_name' in the final dataset denotes the downloaded HTML file name.

### Merging debates and collapsed speeches
The next step, '3_merge_and_clean.ipynb', combines 'hansard_hong_kong.csv' and 'scrape_results.csv'. It cleans and enhances the interpretability of the data, resulting in 'all_speeches.csv' stored in 'intermediate_outputs'.

### Adding person information
Finally, '4_add_person_info.ipynb' incorporates data on people, posts, memberships, and parties from [people.json](https://parser.theyworkforyou.com/members.html), merging it with 'all_speeches.csv' to produce the comprehensive 'all_speeches_and_person.csv'.

## Visualising each party's frequency of discussing Hong Kong
This part creates visualisations of each party's annual frequency of Hong Kong mentions from 2002 to 2023, available in the 'graph_outputs' folder. The start year 2002 is due to [TheyWorkForYou]'s archival limitations on the House of Commons' written answers and written ministerial statements. The visualisations include both overall frequency and normalised frequency per seat to account for variations in party size. While the overall frequency graph includes speeches in both the UK Parliament and regional assemblies, the normalised graph only includes speeches in the UK Parliament.

'5_seat_distribution.ipynb' extracts the UK Parliament's monthly seats distribution from [people.json](https://parser.theyworkforyou.com/members.html). An outstanding issue concerns a slight discrepancy in the total number of seats for the House of Lords compared to official figures. Any insights or corrections regarding this discrepancy are welcome.

## Licensing
Please adhere to [TheyWorkForYou's terms](https://www.theyworkforyou.com/api/terms#licensing) and the [Open Parliament Licence](https://www.parliament.uk/site-information/copyright/) for using the parliamentary data. The repository's code is publicly available and can be adapted for analyses involving keywords other than 'Hong Kong'. Please feel free to utilise these resources in your work.