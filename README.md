# Twitter Sentiment Analysis → Slack Integration

## Overview
End-to-end data pipeline demonstrating **applied NLP for decision support** in operational workflows. Ingests public social media data, performs sentiment analysis, and delivers actionable signals to operational teams via Slack.

**Key capabilities:** Real-time data ingestion, multi-database architecture, containerized deployment, human-in-the-loop design.

## Architecture
```
Twitter API → MongoDB (raw storage) → PostgreSQL (processed data) 
→ Sentiment Analysis → Slack (alerts + human review)
```

**Tech stack:** Python, MongoDB, PostgreSQL, Docker, Twitter API, Slack API, NLTK/TextBlob for sentiment analysis

**Design rationale:**
- MongoDB for flexible ingestion of variable Twitter data structures
- PostgreSQL for structured sentiment analysis results and audit trail
- Docker for reproducible deployment across environments
- Slack integration to embed signals directly into team workflows

## Governance and Responsible Data Use
This project was designed with **practical data governance** applied to a real-world constraint environment:

**Data minimization:** Only public tweets processed; no long-term storage of personal identifiers beyond what was operationally necessary. User data retained only during active processing window.

**Bias awareness:** Sentiment models carry inherent biases (language, cultural context, sarcasm detection). System was designed to **surface signals, not make autonomous decisions** — human review was mandatory before action.

**Platform compliance:** Implemented rate limiting and error handling to respect Twitter API terms of service. System gracefully degraded when rate limits hit rather than circumventing controls.

**Transparency:** Sentiment scores were logged alongside raw text, enabling audit of classification decisions and downstream investigation of false positives/negatives.

## Monitoring and Operational Considerations
**Data quality monitoring:**
- API connection health checks and automatic retry logic
- Detection of unusually low/high sentiment score distributions (signal of model drift or data quality issues)
- Logging of failed API calls, parse errors, and processing exceptions

**Noise reduction:**
- Confidence thresholds to filter ambiguous sentiment classifications
- Configurable alert criteria to prevent notification fatigue
- Designed to be extended with additional validation layers or escalation rules

**Human-in-the-loop design:** System explicitly **does not auto-action** on sentiment results. Slack integration was for awareness and triage, not automated response.

## Limitations and Context
This repository represents a **proof-of-concept implementation** from 2023. Production systems I have designed and built since then include:
- Row-level access controls and audit logging
- Formal data retention and deletion policies
- Stakeholder review processes for model changes
- Incident response procedures for data quality failures

These systems are not publicly available due to **security, compliance, and commercial sensitivity** requirements.

## Use Cases for Human Rights / Nonprofit Context
This architecture pattern is directly applicable to:
- **Community safety monitoring:** Detecting coordinated harassment or threats in public discourse
- **Program evaluation:** Understanding sentiment around policy interventions or advocacy campaigns
- **Early warning systems:** Identifying emerging risks or narratives requiring response
- **Resource allocation:** Prioritizing support based on community-expressed needs

**Critical consideration:** In human rights contexts, false positives and negatives have higher stakes. Any similar system would require **enhanced human oversight, appeal mechanisms, and continuous evaluation for bias** against vulnerable populations.

## Installation & Usage
```bash
# Clone repository
git clone https://github.com/Nathan-Austin/twitter_slackbot_with-sentiment_analysis.git

# Set up environment variables
# Create .env file with:
# TWITTER_API_KEY=your_key
# TWITTER_API_SECRET=your_secret
# SLACK_WEBHOOK_URL=your_webhook
# MONGODB_URI=your_mongodb_connection
# POSTGRES_URI=your_postgres_connection

# Run with Docker
docker-compose up --build
```

## License
MIT License
