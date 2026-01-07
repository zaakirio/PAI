# PAI Setup Guide: Therapy & Language Learning Assistant

A step-by-step guide to set up PAI with custom skills for therapy support and Russian/Arabic/Italian language learning.

---

## Prerequisites

- **Bun runtime**: `curl -fsSL https://bun.sh/install | bash`
- **Claude Code**: Already installed
- **ElevenLabs API key** (optional, for voice): https://elevenlabs.io

---

## Part 1: Install PAI

### Step 1: Clone the Repository

```bash
git clone https://github.com/danielmiessler/PAI.git
cd PAI
```

### Step 2: Run the Bundle Installer

```bash
cd Bundles/Kai
bun run install.ts
```

The installer will prompt for:
- **AI name**: Choose something meaningful (e.g., "Sage", "Mentor", "Guide")
- **Timezone**: Your timezone (e.g., "America/New_York")
- **Voice preference**: Yes if you want TTS

### Step 3: Install Packs (In Order)

Navigate to each pack and tell Claude to install it:

```bash
# Pack 1: Hook System (Foundation)
cd ~/PAI/Packs/kai-hook-system
# In Claude Code: "Read INSTALL.md and install this pack"

# Pack 2: History System (Memory)
cd ~/PAI/Packs/kai-history-system
# In Claude Code: "Read INSTALL.md and install this pack"

# Pack 3: Core Install (Skills & Identity)
cd ~/PAI/Packs/kai-core-install
# In Claude Code: "Read INSTALL.md and install this pack"

# Pack 4: Voice System (Optional - for pronunciation)
cd ~/PAI/Packs/kai-voice-system
# In Claude Code: "Read INSTALL.md and install this pack"
```

### Step 4: Configure Environment

Edit `~/.claude/.env`:

```bash
# Your AI's name
DA=Sage

# Timezone
TIME_ZONE=America/New_York

# ElevenLabs (optional, for voice)
TTS_PROVIDER=elevenlabs
ELEVENLABS_API_KEY=your_api_key_here
ELEVENLABS_VOICE_ID=21m00Tcm4TlvDq8ikWAM
```

### Step 5: Restart Claude Code

Hooks only load on startup. Restart Claude Code to activate PAI.

---

## Part 2: Create Therapy Skill

### Step 1: Create Directory Structure

```bash
mkdir -p ~/.claude/skills/Therapy/Workflows
mkdir -p ~/.claude/skills/Therapy/Data
```

### Step 2: Create SKILL.md

Create `~/.claude/skills/Therapy/SKILL.md`:

```markdown
# Therapy Skill

A supportive companion for mental wellness, journaling, and personal growth.

## Triggers

Activate this skill when user mentions:
- "therapy session", "let's talk"
- "I've been feeling", "I'm struggling with"
- "anxiety", "stress", "depression", "overwhelmed"
- "journal", "reflect", "process my feelings"
- "check in with me", "how am I doing"

## Workflows

- **ActiveListening** - Supportive conversation with reflective questions
- **CognitiveBehavioral** - CBT techniques for reframing negative thoughts
- **JournalPrompts** - Guided journaling exercises
- **ProgressReview** - Review patterns and progress across sessions

## Guidelines

1. **Be supportive, not diagnostic** - You are NOT a replacement for professional mental health care
2. **Remember context** - Reference past sessions from history/sessions/
3. **Encourage professional help** - For serious concerns, always suggest professional support
4. **Maintain boundaries** - Don't pretend to be a licensed therapist
5. **Track patterns** - Note recurring themes in history/learnings/

## Response Format

When in therapy mode:
- Use warm, empathetic language
- Ask open-ended reflective questions
- Summarize what you hear before responding
- Offer gentle insights, not prescriptions
- End with actionable next steps or reflection prompts
```

### Step 3: Create Workflows

**Active Listening Workflow** - `~/.claude/skills/Therapy/Workflows/ActiveListening.md`:

```markdown
# Active Listening Workflow

## Purpose
Provide supportive, non-judgmental space for user to process feelings.

## Steps

1. **Welcome & Check-in**
   - "How are you feeling today?"
   - Note emotional state for history

2. **Listen & Reflect**
   - Let user share without interruption
   - Reflect back: "It sounds like you're feeling..."
   - Validate: "That's completely understandable"

3. **Explore Gently**
   - "Can you tell me more about...?"
   - "When did you first notice this feeling?"
   - "What do you think triggered this?"

4. **Summarize & Support**
   - Summarize key points
   - Acknowledge their strength in sharing
   - Offer perspective if appropriate

5. **Close & Next Steps**
   - "What feels like a small step you could take?"
   - Schedule follow-up if desired
   - Log session to history/sessions/
```

**CBT Workflow** - `~/.claude/skills/Therapy/Workflows/CognitiveBehavioral.md`:

```markdown
# Cognitive Behavioral Workflow

## Purpose
Help identify and reframe negative thought patterns.

## Steps

1. **Identify the Situation**
   - "What happened that's bothering you?"
   - Get specific details

2. **Identify Automatic Thoughts**
   - "What went through your mind when that happened?"
   - "What are you telling yourself about this?"

3. **Identify Cognitive Distortions**
   Common patterns to look for:
   - All-or-nothing thinking
   - Catastrophizing
   - Mind reading
   - Should statements
   - Personalization

4. **Challenge the Thought**
   - "What evidence supports this thought?"
   - "What evidence contradicts it?"
   - "What would you tell a friend in this situation?"

5. **Reframe**
   - Help create balanced alternative thought
   - "A more balanced way to see this might be..."

6. **Record Learning**
   - Log to history/learnings/ with:
     - Original thought
     - Distortion identified
     - Reframed thought
```

**Journal Prompts Workflow** - `~/.claude/skills/Therapy/Workflows/JournalPrompts.md`:

```markdown
# Journal Prompts Workflow

## Purpose
Provide guided journaling exercises for self-reflection.

## Prompt Categories

### Gratitude
- "What are three things you're grateful for today?"
- "Who made a positive difference in your life recently?"

### Self-Reflection
- "What would your younger self think of who you've become?"
- "What's one thing you're proud of this week?"
- "What's a challenge you're facing, and what's it teaching you?"

### Emotional Processing
- "Describe your current emotion as a weather pattern"
- "Write a letter to your anxiety/fear/sadness"
- "What does your inner critic say? Now write a compassionate response"

### Future Self
- "Describe your ideal day one year from now"
- "What would you do if you knew you couldn't fail?"

## Process

1. Ask user what type of reflection they want
2. Offer 2-3 prompt options
3. Let them write freely (don't interrupt)
4. Reflect back themes you notice
5. Save entry to history/sessions/
```

---

## Part 3: Create Language Learning Skill

### Step 1: Create Directory Structure

```bash
mkdir -p ~/.claude/skills/LanguageLearning/Workflows
mkdir -p ~/.claude/skills/LanguageLearning/Data/Russian
mkdir -p ~/.claude/skills/LanguageLearning/Data/Arabic
mkdir -p ~/.claude/skills/LanguageLearning/Data/Italian
```

### Step 2: Create SKILL.md

Create `~/.claude/skills/LanguageLearning/SKILL.md`:

```markdown
# Language Learning Skill

A personalized language tutor for Russian, Arabic, and Italian.

## Languages

- **Russian** (Primary) - Cyrillic script, 6 grammatical cases, aspect pairs
- **Arabic** - Right-to-left script, root system, MSA vs dialects
- **Italian** - Romance language, verb conjugations, formal/informal

## Triggers

Activate when user mentions:
- "teach me Russian/Arabic/Italian"
- "vocabulary", "vocab drill", "new words"
- "conversation practice", "dialogue"
- "grammar", "conjugation", "declension"
- "pronunciation", "how do you say"
- "quiz me", "test me"

## Workflows

- **VocabularyDrill** - Learn new words with spaced repetition
- **ConversationPractice** - Simulated dialogues
- **GrammarExercises** - Targeted grammar practice
- **PronunciationPractice** - Speaking exercises (requires voice)
- **ProgressAssessment** - Weekly/monthly progress review

## Teaching Approach

1. **Context first** - Always teach words in sentences, not isolation
2. **Spaced repetition** - Review words at 1, 3, 7, 14, 30 day intervals
3. **Active recall** - Quiz before showing answers
4. **Error tracking** - Note mistakes in history/learnings/
5. **Celebrate progress** - Acknowledge milestones

## Session Structure

1. **Review** (5 min) - Quiz on previous vocabulary
2. **New Material** (10 min) - Introduce new words/concepts
3. **Practice** (10 min) - Exercises and conversation
4. **Summary** (5 min) - Recap and assign homework
```

### Step 3: Create Workflows

**Vocabulary Drill** - `~/.claude/skills/LanguageLearning/Workflows/VocabularyDrill.md`:

```markdown
# Vocabulary Drill Workflow

## Purpose
Teach new vocabulary using spaced repetition principles.

## Session Format

### 1. Review Previous Words (5 min)
- Check history/learnings/ for words learned in past sessions
- Quiz words due for review (1, 3, 7, 14, 30 days ago)
- Format: Show English → User guesses target language

### 2. Introduce New Words (10 min)
- Present 5-7 new words per session
- For each word:
  - Target language + pronunciation guide
  - English meaning
  - Example sentence in context
  - Memory tip or etymology

### 3. Practice Exercises
- Fill in the blank (target language)
- Translation: English → Target
- Translation: Target → English
- Use in a sentence

### 4. Record Progress
- Log new words to history/learnings/[language]/vocabulary.md
- Note any words that needed multiple attempts

## Language-Specific Notes

### Russian
- Always show: Cyrillic + transliteration + stress mark
- Include gender (м/ж/с) and case patterns
- For verbs: show aspect pair (imperfective/perfective)

Example:
```
слово (slóvo) n. - word
Example: Это новое слово. (This is a new word.)
Cases: слова (gen), слову (dat), словом (inst)
```

### Arabic
- Show: Arabic script + transliteration
- Include root letters (e.g., ك-ت-ب for writing)
- Note MSA vs common dialect differences

Example:
```
كتاب (kitāb) - book
Root: ك-ت-ب (k-t-b) = writing
Plural: كتب (kutub)
```

### Italian
- Include gender (m/f) and plural form
- For verbs: show conjugation pattern (-are, -ere, -ire)
- Note any irregular forms

Example:
```
libro (m) - book
Plural: libri
Example: Ho letto un libro interessante.
```
```

**Conversation Practice** - `~/.claude/skills/LanguageLearning/Workflows/ConversationPractice.md`:

```markdown
# Conversation Practice Workflow

## Purpose
Simulate real conversations to build speaking confidence.

## Scenarios by Level

### Beginner
- Introducing yourself
- Ordering at a restaurant
- Asking for directions
- Shopping for groceries
- Making small talk about weather

### Intermediate
- Describing your job/hobbies
- Making plans with friends
- Discussing a movie/book
- Handling a problem (lost item, wrong order)
- Talking about travel plans

### Advanced
- Debating a topic
- Job interview
- Negotiating a price
- Telling a story about your past
- Discussing current events

## Session Structure

1. **Set the Scene**
   - Describe the scenario
   - Assign roles (user = customer/tourist/etc.)

2. **Dialogue**
   - AI speaks in target language
   - User responds (can ask for help)
   - AI provides corrections gently

3. **Vocabulary Support**
   - User can ask "How do I say...?"
   - Provide phrase, then continue dialogue

4. **Debrief**
   - Review new vocabulary used
   - Note common mistakes
   - Suggest phrases for next time

## Russian Scenario Example

**Scene: Ordering coffee in Moscow**

AI: Здравствуйте! Что будете заказывать?
(Hello! What will you be ordering?)

User: [responds]

AI: [continues dialogue, corrects if needed]
```

**Grammar Exercises** - `~/.claude/skills/LanguageLearning/Workflows/GrammarExercises.md`:

```markdown
# Grammar Exercises Workflow

## Purpose
Targeted practice on specific grammar concepts.

## Russian Grammar Topics

### Cases (Падежи)
1. **Nominative** (Именительный) - subject
2. **Genitive** (Родительный) - possession, absence, quantities
3. **Dative** (Дательный) - indirect object, "to/for someone"
4. **Accusative** (Винительный) - direct object
5. **Instrumental** (Творительный) - "with/by means of"
6. **Prepositional** (Предложный) - location, "about"

### Verb Aspect
- Imperfective: ongoing, repeated, general
- Perfective: completed, single, result-focused

### Exercise Types
- Fill in correct case ending
- Choose imperfective vs perfective
- Transform sentences

## Arabic Grammar Topics

### Root System
- 3-letter roots carry meaning
- Patterns modify meaning (doer, place, tool, etc.)

### Verb Forms
- Form I (basic) through Form X
- Each form has predictable meaning modification

### Exercise Types
- Identify root letters
- Derive words from root
- Conjugate verbs in different forms

## Italian Grammar Topics

### Verb Tenses
- Present (presente)
- Past (passato prossimo, imperfetto)
- Future (futuro semplice)
- Conditional (condizionale)
- Subjunctive (congiuntivo)

### Exercise Types
- Conjugation drills
- Choose correct tense for context
- Transform statements to questions
```

### Step 4: Create Language Data Files

**Russian Reference** - `~/.claude/skills/LanguageLearning/Data/Russian/CaseDeclensions.md`:

```markdown
# Russian Case Declensions Reference

## Masculine Nouns (hard stem)

| Case | Singular | Plural | Question |
|------|----------|--------|----------|
| Nom | стол | столы | кто? что? |
| Gen | стола | столов | кого? чего? |
| Dat | столу | столам | кому? чему? |
| Acc | стол | столы | кого? что? |
| Inst | столом | столами | кем? чем? |
| Prep | столе | столах | о ком? о чём? |

## Feminine Nouns (-а/-я)

| Case | Singular | Plural |
|------|----------|--------|
| Nom | книга | книги |
| Gen | книги | книг |
| Dat | книге | книгам |
| Acc | книгу | книги |
| Inst | книгой | книгами |
| Prep | книге | книгах |

## Common Prepositions by Case

- **Genitive**: без (without), для (for), из (from), от (from), у (at/by)
- **Dative**: к (toward), по (along/by)
- **Accusative**: в/на (into/onto - motion), через (through), за (behind - motion)
- **Instrumental**: с (with), за (behind - location), над (above), под (under)
- **Prepositional**: в/на (in/on - location), о (about)
```

**Arabic Reference** - `~/.claude/skills/LanguageLearning/Data/Arabic/RootSystem.md`:

```markdown
# Arabic Root System Reference

## How Roots Work

Arabic words are built from 3-letter roots. The root carries core meaning, and patterns modify it.

## Example: Root ك-ت-ب (k-t-b) = "writing"

| Word | Transliteration | Pattern | Meaning |
|------|-----------------|---------|---------|
| كَتَبَ | kataba | فَعَلَ | he wrote |
| كِتَاب | kitāb | فِعَال | book |
| كَاتِب | kātib | فَاعِل | writer |
| مَكْتَب | maktab | مَفْعَل | office/desk |
| مَكْتُوب | maktūb | مَفْعُول | written/letter |
| كُتُب | kutub | فُعُل | books |

## Common Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| فَاعِل | doer | كاتب (writer) |
| مَفْعُول | done to | مكتوب (written) |
| مَفْعَل | place | مكتب (office) |
| فِعَالَة | profession | كِتَابَة (writing) |
```

---

## Part 4: Register Skills in CORE

Edit `~/.claude/skills/CORE/SKILL.md` and add:

```markdown
## Available Skills

- **Therapy** - Mental wellness support, journaling, CBT techniques
- **LanguageLearning** - Russian, Arabic, Italian tutoring

## Intent Routing

### Route to Therapy when user mentions:
- Feelings, emotions, struggles
- Anxiety, stress, depression
- Journaling, reflection, check-in
- "How am I doing", "I need to talk"

### Route to LanguageLearning when user mentions:
- Russian, Arabic, Italian
- Vocabulary, grammar, pronunciation
- "Teach me", "Quiz me", "Practice"
- Language-specific terms (Cyrillic, cases, conjugation)
```

---

## Part 5: Test Your Setup

### Test Therapy Skill

```
"I've been feeling overwhelmed lately, can we talk?"
"Give me a journal prompt for self-reflection"
"Help me reframe this negative thought..."
```

### Test Language Learning Skill

```
"Teach me 5 new Russian words about food"
"Let's practice an Italian conversation at a restaurant"
"Quiz me on Arabic root patterns"
"Explain Russian case declensions with examples"
```

### Check History is Working

```bash
# View session logs
ls ~/.claude/history/sessions/

# Search for specific topics
grep -r "Russian" ~/.claude/history/
grep -r "anxiety" ~/.claude/history/
```

---

## Quick Reference

### Directory Structure

```
~/.claude/
├── .env                              # API keys, AI name, timezone
├── settings.json                     # Hook registrations
├── hooks/                            # Event automation
├── skills/
│   ├── CORE/                        # Identity and routing
│   │   └── SKILL.md
│   ├── Therapy/
│   │   ├── SKILL.md
│   │   ├── Workflows/
│   │   │   ├── ActiveListening.md
│   │   │   ├── CognitiveBehavioral.md
│   │   │   └── JournalPrompts.md
│   │   └── Data/
│   └── LanguageLearning/
│       ├── SKILL.md
│       ├── Workflows/
│       │   ├── VocabularyDrill.md
│       │   ├── ConversationPractice.md
│       │   └── GrammarExercises.md
│       └── Data/
│           ├── Russian/
│           ├── Arabic/
│           └── Italian/
├── history/                          # Automatic session logs
│   ├── sessions/
│   └── learnings/
└── voice/                            # Voice server (if installed)
```

### Pack Installation Order

1. `kai-hook-system` - Foundation
2. `kai-history-system` - Memory
3. `kai-core-install` - Skills & Identity
4. `kai-voice-system` - Voice (optional)

### Key Commands

```bash
# View installed hooks
cat ~/.claude/settings.json | grep -A 20 '"hooks"'

# Generate skill index
bun run ~/.claude/tools/GenerateSkillIndex.ts

# Check history
ls -la ~/.claude/history/

# View recent sessions
ls ~/.claude/history/sessions/$(date +%Y-%m)/
```

---

## Troubleshooting

### Hooks not firing
- Restart Claude Code (hooks only load on startup)
- Check `settings.json` has hook entries

### Skills not routing
- Verify SKILL.md exists in skill directory
- Regenerate skill index
- Check CORE/SKILL.md has routing rules

### History not capturing
- Ensure `kai-history-system` is installed
- Check history directory exists: `ls ~/.claude/history/`

### Voice not working
- Verify ElevenLabs API key in `.env`
- Check voice server is running: `curl http://localhost:8888/health`
