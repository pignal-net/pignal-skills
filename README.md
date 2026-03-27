# Pignal Skills

Domain knowledge plugins for operating [Pignal](https://pignal.net) websites with AI agents.

Pignal's MCP server gives your AI agent **tools** — the ability to create, manage, and publish content. These skills give your agent **expertise** — the domain knowledge to use those tools like a professional blogger, shop owner, wiki editor, or any other content specialist.

## Quick Start

```bash
# Add the Pignal marketplace (one-time)
/plugin marketplace add pignalnet/pignal-skills

# Install the plugins you need
/plugin install pignal-platform@pignal-skills    # Site operations (recommended for all)
/plugin install pignal-blogger@pignal-skills      # Blog content expertise
/plugin install pignal-seo@pignal-skills          # SEO optimization
```

Skills auto-trigger based on context. Ask "help me write a blog post" and the blogger skill activates. Ask "deploy my site" and the platform skill takes over.

## Available Plugins

### Platform & Workflow (works with any template)

| Plugin | Skills | What it teaches your agent |
|--------|--------|---------------------------|
| `pignal-platform` | site-ops, site-health | Site lifecycle, MCP orchestration, deployment, monitoring |
| `pignal-seo` | seo-mastery | On-page SEO, structured data, E-E-A-T, LLM readiness |
| `pignal-content-ops` | content-quality | Review methodology, quality gates, content lifecycle |
| `pignal-forms` | form-design | Form UX, lead capture, CTAs, webhook integration |

### Template-Specific Content Expertise

#### Knowledge
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-blogger` | Blog / Signals | Blogging craft, editorial workflow, content strategy |
| `pignal-wiki` | Wiki / Knowledge Base | Technical writing, Diataxis framework, docs-as-code |
| `pignal-til` | Today I Learned | Micro-knowledge capture, atomic ideas |
| `pignal-glossary` | Glossary / Reference | Lexicography, clear definitions |

#### Media
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-magazine` | Magazine / News | Journalism, inverted pyramid, headline craft |
| `pignal-reviews` | Reviews / Ratings | Balanced assessment, rating methodology |
| `pignal-podcast` | Podcast / Episodes | Show notes, episode management |

#### Commerce
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-shop` | Product Catalog | Product copywriting, pricing psychology |
| `pignal-menu` | Menu / Price List | Menu engineering, food description |
| `pignal-services` | Service Directory | Service positioning, tier design |

#### Personal
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-recipes` | Recipe Collection | Recipe writing, structured instructions |
| `pignal-journal` | Journal / Diary | Reflective writing, journaling techniques |
| `pignal-bookshelf` | Reading List | Book review craft, reading tracking |

#### Operations
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-changelog` | Changelog | Keep a Changelog, semver, release notes |
| `pignal-runbook` | Runbook / SOP | Procedural writing, SOP methodology |
| `pignal-incidents` | Incident Log | Incident response, postmortem methodology |

#### Creative
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-portfolio` | Portfolio / Gallery | Project storytelling, impact presentation |
| `pignal-writing` | Writing / Fiction | Narrative structure, creative craft |

#### Education
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-course` | Course / Tutorial | Instructional design, Bloom's taxonomy |
| `pignal-flashcards` | Study Cards | Q&A design, spaced repetition |

#### Community
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-directory` | Resource Directory | Resource curation, evaluation criteria |
| `pignal-awesome` | Curated List | Awesome-list standards, link quality |

#### Professional
| Plugin | Template | Domain expertise |
|--------|----------|-----------------|
| `pignal-case-studies` | Case Studies | Case study methodology, outcome storytelling |
| `pignal-resume` | Resume / CV | Career branding, achievement writing |

## How It Works

Skills teach your AI agent **domain expertise** — the craft knowledge that separates generic content from professional-quality output. They do NOT contain template configuration (types, workspaces, field limits). That information is discovered at runtime via MCP's `get_metadata` tool.

**The workflow:**

1. Skill loads domain expertise into context (stable craft knowledge)
2. Agent calls `get_metadata` via MCP to discover your site's current configuration
3. Agent combines craft knowledge + site context to produce expert-quality content

This separation means skills never go stale when you update your site configuration.

## Recommended Combinations

| If you're running a... | Install these plugins |
|------------------------|----------------------|
| Blog | `pignal-platform` + `pignal-blogger` + `pignal-seo` |
| Online Shop | `pignal-platform` + `pignal-shop` + `pignal-seo` + `pignal-forms` |
| Documentation Site | `pignal-platform` + `pignal-wiki` + `pignal-seo` |
| News/Magazine | `pignal-platform` + `pignal-magazine` + `pignal-seo` |
| Portfolio | `pignal-platform` + `pignal-portfolio` + `pignal-seo` |
| Recipe Site | `pignal-platform` + `pignal-recipes` + `pignal-seo` + `pignal-forms` |
| Course Platform | `pignal-platform` + `pignal-course` + `pignal-seo` |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on contributing new skills or improving existing ones.

## License

MIT
