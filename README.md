# 🤖 Premium AI News Newsletter (Perplexity Plus)

An automated daily AI newsletter workflow powered by n8n, Perplexity AI, and Claude Sonnet 4.5. This workflow curates premium AI news from multiple sources and delivers a beautifully formatted HTML newsletter to your inbox every morning.

## 📋 Overview

This workflow aggregates AI news from **6 premium sources** to create a balanced, professional newsletter featuring:

- **📰 Breaking News** - Real-time AI industry updates via Perplexity
- **🔬 Research Papers** - Latest arXiv AI research
- **📢 Official Announcements** - Industry blogs and academic breakthroughs
- **💼 Industry News** - Product launches, funding, company updates
- **🚀 Applications** - Real-world AI implementations and case studies

### Key Features

✨ **AI-Powered Curation** - Claude Sonnet 4.5 selects and summarizes the most significant stories
🎯 **Balanced Content** - Automatically ensures diversity across news, research, and announcements
🔄 **Smart Deduplication** - Uses Levenshtein distance to detect and filter duplicate stories
📧 **Beautiful HTML Emails** - Professional newsletter design with color-coded badges
⏰ **Daily Automation** - Runs every morning at 5:00 AM
💾 **State Management** - Tracks sent articles to prevent duplicates (30-day retention)

## 🏗️ Architecture

### Workflow Structure

```
Trigger (Daily 5AM)
    ├─→ arXiv Research (5 papers)
    ├─→ DeepMind Blog (RSS)
    ├─→ Perplexity Breaking News (10 stories)
    ├─→ Perplexity Industry News (5 stories)
    ├─→ Perplexity Research News (5 stories)
    └─→ Perplexity Applications (5 stories)
            ↓
    Merge All Sources (6 inputs)
            ↓
    Deduplicate & Prioritize (news: 3, announcement: 2, research: 1)
            ↓
    AI News Curator (Claude Sonnet 4.5 with Structured Output)
            ↓
    Generate Premium HTML
            ↓
    Send via Gmail
```

### Content Mix

The workflow is optimized for a **balanced content distribution**:

- **40% News** (~10-15 Perplexity articles prioritized)
- **30% Announcements** (DeepMind + Perplexity Research News)
- **30% Research** (5 arXiv papers)

Each newsletter features **8-10 carefully curated stories** with:
- Compelling headlines
- Impact assessment (High/Medium)
- Professional summaries explaining relevance to AI practitioners
- Source attribution and direct links

## 🚀 Setup Instructions

### Prerequisites

1. **n8n instance** (self-hosted or cloud)
2. **API Keys**:
   - [Perplexity API](https://www.perplexity.ai/) (for news aggregation)
   - [Anthropic API](https://www.anthropic.com/) (for Claude Sonnet 4.5)
3. **Gmail OAuth2** credentials (or alternative email service)

### Installation

1. **Import the workflow**:
   - Download `Premium AI News Newsletter (Native Nodes) v2.json`
   - In n8n, go to **Workflows** → **Import from File**
   - Select the downloaded JSON file

2. **Configure Credentials**:

   **Perplexity API** (4 nodes require this):
   - Go to n8n **Credentials** → **Add Credential** → **Perplexity API**
   - Enter your Perplexity API key
   - Replace `YOUR_PERPLEXITY_CREDENTIAL_ID` in:
     - Perplexity Breaking News
     - Perplexity Industry News
     - Perplexity Research News
     - Perplexity Applications

   **Anthropic API**:
   - Go to n8n **Credentials** → **Add Credential** → **Anthropic API**
   - Enter your Anthropic API key
   - Replace `YOUR_ANTHROPIC_CREDENTIAL_ID` in **Claude Sonnet 4.5** node

   **Gmail OAuth2**:
   - Go to n8n **Credentials** → **Add Credential** → **Gmail OAuth2**
   - Follow the OAuth flow to authorize
   - Replace `YOUR_GMAIL_CREDENTIAL_ID` in **Send via Gmail** node

3. **Update Email Address**:
   - Open the **Send via Gmail** node
   - Replace `your-email@example.com` with your email address

4. **Test the Workflow**:
   - Click **Execute Workflow** to run a test
   - Check each node's output to ensure data flows correctly
   - Verify you receive the newsletter email

5. **Activate**:
   - Toggle the workflow to **Active**
   - It will now run daily at 5:00 AM

### Customization Options

#### Adjust Newsletter Timing
Edit the **Daily at 5:00 AM** node cron expression:
```javascript
"0 5 * * *"  // Default: 5:00 AM daily
"0 8 * * 1-5"  // Example: 8:00 AM weekdays only
```

#### Change Content Volume
- **arXiv**: Modify `max_results` in **Fetch arXiv XML** (currently 5)
- **Perplexity**: Adjust prompts to request more/fewer articles (currently 5-10 each)

#### Adjust Priority Weights
Edit the **Deduplicate & Prioritize** node:
```javascript
const typePriority = { news: 3, announcement: 2, research: 1 };
// Higher number = higher priority
```

#### Customize AI Curation
Edit the **AI News Curator** prompt to:
- Change story count (currently 8-10)
- Adjust content balance (currently 2-3 news, 2-3 announcements, 3-4 research)
- Modify tone or style preferences

## 📊 Node Details

| Node | Type | Purpose |
|------|------|---------|
| Daily at 5:00 AM | Schedule Trigger | Runs workflow daily |
| Fetch arXiv XML | HTTP Request | Fetches latest AI research papers |
| Parse arXiv XML | XML Parser | Converts XML to JSON |
| Format arXiv Data | Code | Normalizes arXiv data structure |
| DeepMind Blog | RSS Feed Read | Reads DeepMind announcements |
| Format DeepMind Data | Code | Normalizes RSS data |
| Perplexity Breaking News | Perplexity | General AI news |
| Perplexity Industry News | Perplexity | Product/company news |
| Perplexity Research News | Perplexity | Research breakthroughs |
| Perplexity Applications | Perplexity | Real-world AI applications |
| Format [Source] Data | Code (×4) | Normalizes Perplexity responses |
| Combine All Sources | Merge | Combines 6 data streams |
| Deduplicate & Prioritize | Code | Removes duplicates, sorts by priority |
| AI News Curator | LangChain LLM | Claude selects & summarizes stories |
| Claude Sonnet 4.5 | Anthropic Model | AI language model |
| Structured Output Parser | LangChain | Ensures valid JSON output |
| Generate Premium HTML | Code | Creates newsletter HTML |
| Send via Gmail | Gmail | Delivers email |

## 🎨 Newsletter Design

The generated newsletter features:

- **Responsive HTML** - Mobile-optimized design
- **Color-coded badges**:
  - 🔬 Research (Purple)
  - 📢 Announcement (Blue)
  - 📰 News (Green)
- **Impact indicators** - High (Red) or Medium (Orange)
- **Professional typography** - Sans-serif, hierarchical layout
- **Gradient header** - Purple-to-violet brand aesthetic
- **Editor's Note** - AI-generated daily theme summary

## 🔧 Troubleshooting

### No newsletter received
- Check workflow execution history for errors
- Verify all credentials are properly configured
- Ensure the workflow is set to **Active**
- Check Gmail spam/promotions folder

### Missing content from specific sources
- Verify API credentials are valid
- Check node execution output for errors
- RSS feeds may occasionally be unavailable (nodes have `continueOnFail: true`)

### Duplicate articles appearing
- The deduplication logic tracks articles for 30 days
- Clear workflow static data to reset: Settings → Static Data → Clear

### AI curation issues
- Verify Anthropic API key is valid
- Check Claude Sonnet 4.5 node for error messages
- Structured Output Parser ensures valid JSON - check for parsing errors

## 📄 License

This workflow is provided as-is for personal and commercial use. Attribution appreciated but not required.

## 🤝 Contributing

Found a bug or have an enhancement idea?

1. Fork this repository
2. Make your changes
3. Submit a pull request

## 🙏 Acknowledgments

- **n8n** - Open-source workflow automation
- **Perplexity AI** - Real-time news aggregation
- **Anthropic** - Claude Sonnet 4.5 AI curation
- **arXiv** - Open-access research repository
- **DeepMind** - AI research announcements

## 📞 Support

For questions or issues:
- Open an issue on GitHub
- Check n8n community forums
- Review n8n documentation at [docs.n8n.io](https://docs.n8n.io)

---

**Built with ❤️ using n8n, Perplexity, and Claude**
