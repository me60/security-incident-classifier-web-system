<!-- Abstracted HTML Representation -->
<template>
  <div id="app">
    <Header />
    <Tables v-bind:tables="tables"/>
    <Results />
  </div>
</template>

<!-- Body of single page -->
<script>
  import Tables from './components/Tables.vue'
  import Header from './components/Header.vue'
  import Results from './components/Results.vue'
  export default {
    name: 'App',
    components: {
      Tables,
      Header,
      Results
    },
    data() {
      return {
        tables: [
          {name: 'Risk Heat Map', questions: [
            {query: "Which of the following best classifies the incident?", answers: [
              {answer_text: "Malicious Code"},
              {answer_text: "Internal Investigation"},
              {answer_text: "Copyright Infringement"},
              {answer_text: "Denial of Service"},
              {answer_text: "Unauthorised Access"},
              {answer_text: "APT (Advanced Persistent Threat)"},
              {answer_text: "Social (?)"},
              {answer_text: "Threat/Extortion/Blackmail"}
            ]},
            {query: "Is the incident ongoing or over?", answers: [
              {answer_text: "Over"},
              {answer_text: "Ongoing"},
              {answer_text: "Unknown"}
            ]},
            {query: "Is there a potential economic cost to the incident?", answers: [
              {answer_text: "No"},
              {answer_text: "Up to £10,000"},
              {answer_text: "Between £10,000 and £100,000"},
              {answer_text: "Over £100,000"}
            ]},
            {query: "How many people are involved? (does not include data breach)", answers: [
              {answer_text: "0"},
              {answer_text: "1 - 5"},
              {answer_text: "6 - 10"},
              {answer_text: "More than 10"}
            ]},
            {query: "Are we being asked to provide investigation data to an external party?", answers: [
              {answer_text: "Yes"},
              {answer_text: "No"},
              {answer_text: "N/A or Don't Know"}
            ]},
            {query: "Have we lost access to data?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "Are back-ups available?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "Is this a critical time in the calendar for those involved?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "If a system is affected, can business continue without it?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "If a system is affected, is the system critical to the functioning of the Uni?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "Does this have potential to spread automatically?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "Does this feel like a major incident?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]},
            {query: "Are VIPs affected?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"},
							{answer_text: "N/A or Don't Know"}
						]}
          ]},
          {name: 'Personal Data', questions: [
            {query: "Can the data identify a living individual?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Does the data include any of the following?"},
            {query: "Racial or Ethnic origin", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Political opinion", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Religious beliefs", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Trade Union membership", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Physical or mental health conditions", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Sex life", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Involvement in criminal proceedings", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Outcomes of criminal convictions", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Is there a substantial risk to health, safety or well being to individuals or groups?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Does it involve the prevention or detection of crime?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Might it stop the apprehension or prosecution of an offender?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Does the data include opinions about an individual?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Is the loss of data likely to cause harm or inconvenience to an individual?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Staff contact details (where these do not concern public facing roles)?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Student/staff photograph?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Contracts of employment?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Student transcripts?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Counselling records?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Disciplinary proceedings?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Grievance proceedings?", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]}
          ]},
          {name: 'University Confidential Data', questions: [
            {query: "Does the information lost include any of the following:"},
            {query: "Course information", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Degree congregation programme (including list of graduates)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Map of University buildings", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Opening hours", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Press releases", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Published research papers", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "University prospectus", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Budget information", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Details of funding settlements", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Draft documents (which do not contain personal or sensitive personal data)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Internal audit reports", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Internal memos", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Key performance indicators", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Lecture materials", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Minutes of University and School committees", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Planning applications", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Student statistics (anonymised)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Commercial contracts", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Disaster recovery / business continuity plans", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Documentation that contains decisions surrounding academic performance", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Examination results", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Payroll / banking details", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Planning / forecasting reports", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Procurement / invitation to tender documentation", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Research grant applications", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Strategic planning", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "University Risk Register and controls", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Accident reports", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Bank/credit card details", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Case files and correspondence surrounding investigations by a regulatory body", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Communications with Government (Ministerial level)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Government (Ministerial level)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Communications with legal counsel", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Communications with Police Scotland (operational matters)", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Legal proceedings", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
            {query: "Passwords and other forms of access control credentials", answers: [
							{answer_text: "Yes"},
							{answer_text: "No"}
						]},
          ]}
        ]
      }
    }
  }
</script>

<!-- Global Styling -->
<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  h3 {
    font-size: 48px;
  }
  h4 {
    font-weight: bold;
    font-style: italic;
  }
</style>
