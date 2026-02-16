# Requirements Document

## Project Title
**Vidya AI** - Multilingual AI Education Assistant for Rural & Underserved Communities

## Challenge Track
**[Student Track] AI for Communities, Access & Public Impact**

---

## 1. Why This Matters (Personal Context)

Last year, I spent a week in my cousin's village school in Maharashtra. What I saw was eye-opening - not in a good way. The school had 300+ students but only 6 teachers. The "computer lab" had 4 PCs from 2010 that barely worked. Smart kids were dropping out because they couldn't understand English textbooks, and the one Math teacher was overwhelmed.

The problem isn't lack of talent or motivation. It's access. While urban kids have YouTube tutorials, online courses, and private tutors, rural students have none of that. Vidya AI is my attempt to level this playing field.

---

## 2. Problem Statement (The Reality)

### 2.1 What's Actually Happening
- **Language Barrier**: In the village schools I visited, 90%+ conversations happen in Marathi/Hindi. But all "quality" content - videos, books, online resources - is in English. Students are learning by rote without understanding.

- **Teacher Shortage**: I saw 1 teacher handling 65 students in Grade 8. Personalized attention? Forget it. Teacher barely has time to complete syllabus.

- **Resource Gap**: No library. No lab equipment. No internet in classrooms. One student told me he watches educational videos by climbing a hill to get signal.

- **Digital Divide**: Forget laptops - most families share one smartphone. Apps need to work on these basic devices with limited data.

- **Drop-out Crisis**: By Grade 9, half the class from Grade 6 is gone. Why? They fell behind, couldn't catch up, gave up.

### 2.2 The Scale of This Problem
- **250+ million** rural students in India (source: UDISE+ 2023-24 data)
- **22 official languages** + 100+ dialects. Education can't be one-size-fits-all.
- Only **12% of rural students** reach college vs 35% urban students (NSO survey)
- **1.2 million teacher positions vacant** across India (RTI response, 2024)

---

## 3. What We're Building (Solution)

### 3.1 Core Features (MVP Focus)

#### 3.1.1 Multilingual AI Tutor
**What it does:**
- Student asks doubt in Hindi/Tamil/Telugu/any Indic language
- AI understands context and explains concept in same language
- Not just translation - actual explanation in their mother tongue

**Languages (phased approach):**
- Phase 1: Hindi, Tamil, English (3 most needed based on UDISE data)
- Phase 2: Telugu, Marathi, Bengali, Gujarati, Kannada
- Phase 3: Malayalam, Odia, Punjabi, Assamese, others

**Technical approach:**
- Using IndicBERT for language understanding
- Fine-tuned on educational content (NCERT, state boards)
- Context-aware responses (knows what chapter student is on)

#### 3.1.2 Voice-First Interface
**Why voice:**
Many students type slowly. Speaking is natural. Voice removes barriers.

**How it works:**
- Student speaks question in any language
- Speech-to-text converts (using Whisper or IndicWav2Vec)
- AI processes and responds
- Text-to-speech reads answer (if student prefers audio)

**Reality check:**
Speech recognition won't be perfect initially. Indian accents, dialects, background noise - lots of challenges. Starting with 85-90% accuracy target, will improve iteratively.

#### 3.1.3 Offline-First (This is Critical)
**Why offline matters:**
Internet is expensive and unreliable in villages. App must work without constant connectivity.

**How we're doing this:**
- Download lessons/chapters when WiFi available (school, home)
- All content cached locally in SQLite database
- Basic AI responses work offline (using on-device models)
- Complex queries queued, processed when connected
- Smart sync: only upload/download changes, not everything

**Technical challenge:**
AI models are huge. Running GPT-style models on phone is impossible. Solution: lightweight models (DistilBERT, quantized versions) + hybrid approach (simple queries on-device, complex ones on server).
- AI-generated explanations for complex concepts
- Visual learning aids (diagrams, animations, videos)
- Practice problems with step-by-step solutions
- Real-time doubt resolution through conversational AI

#### 3.1.4 Offline-First Architecture
- Core functionality available without continuous internet
- Content synchronization during connectivity windows
- Lightweight mobile app (<50MB) for low-end smartphones
- Progressive web app (PWA) for browser access

#### 3.1.5 Assessment & Feedback
- Automated homework checking and grading
- Formative and summative assessments
- Detailed performance reports for students and parents
- Identification of learning patterns and recommendations

#### 3.1.4 Simple Assessment & Practice
**Why:**
Students need to practice and test themselves. Teachers need to track who's struggling.

**What we're building:**
- Auto-generated quizzes based on what student is learning
- Instant feedback (not just "wrong" but "here's why it's wrong")
- Progress tracking - what topics are strong, what needs work
- Weekly/monthly reports for parents (in their language!)

**Starting simple:**
MCQs and fill-in-the-blanks initially. Advanced problem-solving later.

---

## 3.2 What We Learned from Teachers & Students

Before writing this, I talked to 8 teachers and 15 students from 3 different schools (2 rural, 1 urban low-income). Here's what they said:

### Students Want:
- "Explain in Marathi, not English" - Every single student
- "Can I ask same question again?" - They're afraid to repeatedly ask teachers
- "Works without internet?" - Top concern
- "Will it help in exams?" - They're practical, want results
- "Is it free?" - Obvious but important

### Teachers Want:
- "Don't replace us" - Fear of job loss is real, address it upfront
- "Show me who needs help" - They want to identify struggling students
- "Less admin work" - Tired of attendance sheets, grading, etc.
- "NCERT/State board aligned" - Random content won't help
- "Simple to use" - Teachers also struggle with complex apps

### Parents Want:
- "Is child learning?" - Progress visibility
- "Safe for kids?" - Content moderation, privacy concerns
- "Will they study or play games?" - Need usage monitoring
- "Can I understand it?" - Reports in their language

These inputs shaped everything below.

---

## 4. Functional Requirements (What App Must Do)

### 4.1 User Management (Keeping it Simple)
- **Must have**: Phone number-based registration (OTP for verification)
- **Optional**: Aadhaar linking (only if government partnership needs it)
- **Roles**: Student, Teacher, Parent (Admin can come later)
- **Profile**: Name, grade, board, preferred language, subjects
- **Auth**: OTP for first time, then 4-digit PIN for offline access

**Why phone-based?**
Everyone has a phone number. Email? Not so common in villages.

### 4.2 Content Requirements
**Priority 1 (MVP):**
- CBSE curriculum for Grades 6-10
- Math, Science (Physics, Chemistry, Biology), Social Science
- 3 languages: Hindi, Tamil, English
- Mix of text, images, simple animations

**Priority 2:**
- State boards (Maharashtra, Tamil Nadu, UP)
- More subjects (add languages like Sanskrit)
- 5 more Indic languages
- Videos (bandwidth intensive, so optional downloads)

**Content sources:**
- NCERT textbooks (public domain)
- Teacher-contributed content (with review)
- Partnering with content creators (if funding allows)

**Reality check:**
Creating quality curriculum-aligned content in multiple languages is HARD and expensive. Starting focused, expanding gradually.

### 4.3 AI Capabilities (Being Realistic)
**What AI should do:**
- Understand questions in Indic languages (85%+ accuracy target)
- Answer from curriculum content (not make up stuff)
- Explain concepts simply (like a good teacher would)
- Track what student knows, what they don't
- Recommend practice problems

**What AI won't do (at least initially):**
- Replace teachers (not the goal!)
- Grade long essay answers (too complex for MVP)
- Handle every possible question (will have "I don't know, ask your teacher" fallback)

**Technical approach:**
- Question Understanding: IndicBERT fine-tuned on educational Q&A
- Answer Generation: Retrieval-based first (safer than generative), generative later
- Personalization: Simple rule-based initially, ML-based after collecting data
- Fallback: If AI confidence <70%, show related content instead of wrong answer

### 4.4 Voice & Text Interface
**Voice features:**
- Speak question → Get answer (in same language)
- Speech-to-text for Indic languages
- Text-to-speech for reading answers

**Target accuracy:** 90% for speech recognition (realistic for MVP)
**Acceptable:** 85% is okay if we improve iteratively
**Aspiration:** 95%+ (needs more data, fine-tuning)

**Text interface:**\n- Simple chat-like interface (everyone knows WhatsApp)
- Support for Indic scripts (Devanagari, Tamil, Telugu, etc.)
- Big fonts, high contrast (many users have basic phones)
- Voice typing option (easier than typing in Tamil/Hindi)

### 4.5 Offline Mode (Non-Negotiable)
**Must work offline:**
- View downloaded lessons
- Practice problems
- Take quizzes
- Basic AI Q&A (using on-device lightweight model)

**Sync when online:**
- Upload progress, quiz responses
- Download new content
- Update AI model (if there's a newer version)
- Smart sync: only changes, not full data

**Storage management:**
- User controls what to download (like Netflix)
- Auto-delete old content if storage low
- Show chapter size before download
- Compress content (images as WebP, videos as H.265)

**Technical challenge:**
Full AI models need 2-5 GB. Phones have limited storage. Solution: Hybrid - lightweight model (200-500 MB) on device for basic queries, complex ones queued for server.

---

## 5. Non-Functional Requirements (The Hard Part)\n\n### 5.1 Performance (Ground Reality)
**Device target:** Phones with 2GB RAM, Android 8+
- Why? Most affordable phones in ₹5,000-10,000 range have these specs
- Tested on: Redmi 9A, Realme C11, Samsung Galaxy A03\n\n**Performance targets:**
- App launch: <3 seconds (measured on Redmi 9A)
- Question response: <2 seconds if online, <1 second offline
- Content load: <2 seconds for text, <5 seconds for images
- Video playback: Should stream smoothly on 2G/3G (adaptive bitrate)

**Network reality:**
- Many areas have 2G only
- 3G is common, 4G is spotty
- WiFi at school/home is rare\n\n**Data usage:** <10MB per day for active usage (2-3 hours)
- Why? Many users have 1.5GB/month plans (₹50-100)\n- How? Aggressive content compression, smart caching

### 5.2 Security & Privacy (Parents Care About This)

**What data we collect:**
- Name, grade, school (optional), phone number
- Learning progress, quiz scores
- App usage patterns
- Voice recordings (only if user allows, for improving speech recognition)

**What we DON'T collect:**
- Location tracking (unnecessary)
- Personal photos/contacts
- Financial information
- Browsing history outside app

**Data protection:**
- All data encrypted (AES-256)
- Minimal data collection (only what's needed)
- User can delete account anytime
- No selling data to third parties (NEVER)
- DPDPA 2023 compliant (India's data protection law)

**For children (under 18):**
- Parental consent required
- Extra privacy protections
- No targeted ads
- Safe content only (moderated)

**Being honest:**
Building AI requires data. User data helps improve models. But we'll be transparent about what we collect and why. Users can opt-out of data sharing for research.

---

## 6. Technical Constraints (Reality Check)

### 6.1 Budget Constraints
**Year 1 realistic budget:** ₹40-60 lakhs
- Team salaries: ₹25-35 lakhs (5-6 people, mix of full-time + contractors)
- Cloud infrastructure: ₹5-8 lakhs (AWS/GCP, initially free tier, then scales)
- Content creation: ₹8-12 lakhs (teachers, translators, reviewers)
- Miscellaneous: ₹2-5 lakhs (tools, licenses, legal, testing)

**Bootstrap approach:**
- Use open-source models (save millions on model development)
- Start with AWS credits (startups get $25K-100K credits)
- Volunteer interns/students for initial development
- Partner with NGOs for content and field testing

### 6.2 Technical Limitations We Accept

**Can't build everything immediately:**
- ❌ Advanced features like AR/VR (expensive, not priority)
- ❌ Real-time video tutoring (bandwidth + cost prohibitive)
- ❌ Covering all subjects + all grades + all languages (phased approach)
- ❌ Perfect AI accuracy from day 1 (improves iteratively)

**What we're optimizing for:**
- ✅ Works on basic smartphones
- ✅ Functions offline reliably
- ✅ Core learning experience is solid
- ✅ Useful even if not perfect

### 6.3 Scaling Challenges

**Infrastructure:**
- **Problem:** AI inference is expensive (₹2-5 per 1000 requests)
- **Solution:** Tier-based - simple queries use cheap models, complex ones use GPT-4 class models

**Content:**
- **Problem:** Quality content creation doesn't scale easily
- **Solution:** Community contribution model (like Wikipedia) + expert review

**Users:**
- **Problem:** Supporting millions of users requires massive infrastructure
- **Solution:** Start small (1000 users), scale gradually, optimize continuously

**Languages:**
- **Problem:** Some Indian languages have very limited ML resources
- **Solution:** Focus on top 8-10 languages first (covers 85%+ users), others via community contribution

---

## 7. How We'll Know If This Works (Success Metrics)\n\n### What Really Matters:
1. **Are students learning better?**
   - Measure: Pre/post test scores, exam results
   - Target: 20-30% improvement in test scores over 6 months
   - How: Compare with control group (students not using app)

2. **Do they actually use it?**
   - Measure: Daily/weekly active users, time spent\n   - Target: 40%+ students use it at least 3 times/week
   - Reality: Many apps are downloaded but never used

3. **Does it reduce dropouts?**
   - Measure: Student retention in pilot schools
   - Target: 20-30% reduction in dropout rate
   - This is the ultimate goal

4. **Do teachers find it useful?**
   - Measure: Teacher feedback, adoption in classroom
   - Target: 70%+ teachers actively use dashboard features
   - If teachers don't buy in, students won't either

### Technical Metrics (Important but Secondary):
- App shouldn't crash (target: <2% crash rate)
- Should be fast (response time <2-3 seconds)
- Speech recognition should work (target: 90%+ accuracy)
- Should work offline smoothly

### The Honest Truth:
Success isn't just numbers. It's when a student from a village school understands quadratic equations in Tamil, scores well in exams, and continues studying. That's what we're optimizing for.

---

## 8. Risks & What Could Go Wrong (Being Realistic)

| Risk | Probability | Why It's a Risk | What We're Doing |
|------|-------------|-----------------|------------------|
| Users don't adopt it | High | Resistance to new technology, habit change is hard | Start with pilot schools, teacher training, demo sessions |
| AI gives wrong answers | Medium | Models aren't perfect, hallucinations happen | Human review of answers, confidence thresholds, feedback loop |
| Can't create quality content fast enough | High | Content is expensive and time-consuming | Partner with teachers, community contribution, start narrow |
| Runs out of funding | Medium | Startups often die due to cash burn | Bootstrap initially, apply for grants, find partners early |
| Speech recognition doesn't work well | Medium | Indian accents + dialects + noise | Accept 85-90% initially, improve iteratively, text fallback |
| Competition from big players | Low-Medium | Google, BYJU'S, etc. have resources | Focus on rural + local languages (they focus on urban/English) |
| Privacy concerns/backlash | Low | Parents wor ried about data | Transparent policy, minimal data collection, local storage option |

**Mitigation Strategy:**
Start small, fail fast, learn, adapt. If something isn't working in pilot, pivot before scaling.

---

## 9. Timeline (Realistic, Not Optimistic)

### Month 1-3: MVP Development
- **What:** Basic Android app, core AI, 3 languages, minimal content
- **Team:** 2-3 developers, 1 ML engineer, 1 content person
- **Deliverable:** Working prototype that can demo to schools
- **Risks:** Scope creep (want to add too much), technical blockers

### Month 4-6: Beta Testing
- **What:** Pilot with 2-3 schools (100-200 students)
- **Focus:** Gather feedback, fix bugs, improve UX
- **Deliverable:** Stable app with real user data
- **Risks:** Schools might not cooperate, students might not use it

### Month 7-9: Refinement & Expansion
- **What:** Add more languages, offline mode, content
- **Based on:** Feedback from beta users
- **Deliverable:** Production-ready v1.0
- **Risks:** Trying to do too much, losing focus

### Month 10-12: Controlled Launch
- **What:** 10 schools, 1000+ students, teacher training
- **Goal:** Prove impact (learning outcomes data)
- **Deliverable:** Case studies, metrics, testimonials
- **Risks:** Can't scale infrastructure, support overwhelmed

### Year 2: Scale or Pivot\nBased on Year 1 results, either scale aggressively or pivot based on learnings.

**Honest assessment:** Building good edtech is slow. Anyone promising 1M users in 6 months is either overly optimistic or lying. We're targeting 5K-10K active users by end of Year 1, which if done right with real impact, is a solid foundation.

---

## 10. What We Need to Succeed

### Team (Most Critical):
- **ML Engineer**: Someone who knows NLP, has worked with Indic languages
- **Mobile Developer**: React Native + Android optimization experience
- **Backend Developer**: APIs, databases, scaling
- **Content Lead**: Teacher background, knows curriculum
- **Designer/UX**: Simple interfaces for low digital literacy users

### Resources:
- **Cloud Credits**: AWS/GCP startup credits ($25K-50K)
- **Compute**: GPU instances for model training/fine-tuning
- **Testing Devices**: 10-15 low-end Android phones (₹50K-100K)
- **Field Access**: Partnership with NGO/school network

### Partnerships:
- **NGOs**: For reach, credibility, field testing (Pratham, Akshara Foundation)
- **Educators**: Content creation, curriculum alignment
- **Government**: State education boards, DIKSHA integration (long-term)
- **Academic**: Research collaboration, model development

### Funding:
- **Year 1**: ₹40-60 lakhs (can bootstrap with grants + competitions)
- **Year 2**: ₹1-2 crores (need VC/government funding for scale)

---

## 11. Final Thoughts

This isn't just a hackathon project. If it works, it could help millions of students. But I'm under no illusion - this is hard. Building good edtech is harder than building another food delivery app.

**Why I think this can work:**
1. Real problem I've witnessed firsthand
2. Technology is finally mature enough (Indic NLP, affordable smartphones, Jio internet)
3. Government support (NEP 2020, Digital India initiatives)
4. Timing is right post-COVID (online education acceptance increased)

**What's different about this:**
Most edtech focuses on urban, English-speaking, paying customers. We're targeting the opposite - rural, vernacular, affordable. It's a harder problem but much bigger impact.

**Commitment:**
If selected for this hackathon, I'm committed to building this. Not just for the competition, but to actually solve the problem. Looking for co-founders, mentors, partners who share this vision.

---

**Document Info:**
- **Version**: 1.0
- **Created**: February 16, 2026
- **Last Updated**: February 16, 2026
- **For**: AI for Bharat Hackathon 2026
- **Track**: Student Track - AI for Communities, Access & Public Impact
- **Contact**: Available in GitHub repo
