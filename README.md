# SELF-DISCOVERY — Find Yourself. Build Your Path.

Production-ready web application ที่ช่วยให้วัยรุ่นค้นหาตัวเองผ่านคำถามเรื่องเล่า (Digital Storybook), Reflection, ถอดรหัสบุคลิกภาพ และเปลี่ยนสิ่งที่ค้นพบให้เป็น **“7-Day Action Roadmap”** ที่ทำได้จริง

> **Core Feeling:** Digital Storybook ที่ผู้ใช้เป็นตัวละครหลัก  
> **Final Message:** “ฉันยังไม่จำเป็นต้องรู้อนาคตทั้งหมด แค่รู้ว่าก้าวต่อไปของฉันคืออะไร”

---

## 1. Tech Stack

- **Frontend:** React, Vite, TypeScript, Custom CSS & Design System
- **Deployment:** Vercel (Static Build + Vercel Serverless Functions)
- **Backend API:** Vercel Functions (`/api/*`)
- **Database:** PostgreSQL / Supabase (Local-First fallback enabled)
- **MCP Integration:** Model Context Protocol Server (`/api/mcp`) for AI Agents & Developer Tools

---

## 2. Architecture & Data Flow

```text
[ Browser (React + Vite) ]
        │ LocalStorage Primary State (Local-First)
        ▼
[ Background Sync ]
        │ Anonymous Session JSON
        ▼
[ Vercel API Routes / Functions ]
        │
        ▼
[ Database (Supabase / Postgres) ]
```

### MCP Architecture (Model Context Protocol)

MCP ถูกออกแบบแยกจาก Browser Application อย่างเด็ดขาด:

```text
[ AI / Developer Agent ]
          │
          ▼
     [ MCP Client ]
          │ (JSON-RPC / Auth Token)
          ▼
   [ MCP Server (/api/mcp) ]
          │
          ▼
 [ Application API & Database ]
```

---

## 3. Data Model

- `sessions`: `id`, `anonymous_session_id`, `current_step`, `intent`, `archetype`, `choice`, `created_at`, `updated_at`
- `question_responses`: `id`, `session_id`, `question_id`, `answer_id`, `signals`, `created_at`
- `personality_results`: `id`, `session_id`, `primary_archetype`, `secondary_traits`, `signal_scores`, `strengths`, `thinking_style`, `blind_spot`, `environment`
- `characters`: `id`, `session_id`, `archetype`, `character_name`, `story_role`
- `stories`: `id`, `session_id`, `story_version`, `story_data`
- `obstacles`: `id`, `session_id`, `obstacle_type`, `title`, `reflection_question`
- `tarot_draws`: `id`, `session_id`, `card_id`, `card_name`, `keyword`, `reflection_question`
- `reflections`: `id`, `session_id`, `reflection_text`, `created_at`
- `choices`: `id`, `session_id`, `choice`, `created_at`
- `roadmaps`: `id`, `session_id`, `choice`, `days`, `expected_outcome`
- `analytics_events`: `id`, `session_id`, `event_name`, `step`, `metadata`, `created_at`

---

## 4. Local Development

```bash
# 1. Install dependencies
npm install

# 2. Run local development server
npm run dev

# 3. Build for production test
npm run build

# 4. Preview production build
npm run preview
```

---

## 5. Environment Variables

คัดลอกไฟล์ `.env.example` เป็น `.env`:

```text
DATABASE_URL=postgresql://user:password@localhost:5432/self_discovery
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-supabase-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-supabase-service-role-key

MCP_SERVER_URL=https://your-app.vercel.app/api/mcp
MCP_AUTH_TOKEN=your-secure-mcp-auth-token
```

---

## 6. Vercel Deployment

โปรเจกต์นี้ตั้งค่าพร้อมสำหรับการ Deploy บน Vercel:

1. Push โค้ดไปยัง GitHub / GitLab
2. นำเข้า Repository บน Vercel Dashboard
3. กำหนด Environment Variables ตาม `.env.example`
4. กด **Deploy**

หรือใช้ Vercel CLI:
```bash
vercel
```

---

## 7. MCP Setup & Tools

MCP Endpoint อยู่ที่ `/api/mcp` รองรับเครื่องมือดังนี้:

### READ Tools:
- `list_questions`, `get_question`
- `list_archetypes`, `get_archetype`
- `list_tarot_cards`, `get_tarot_card`
- `list_obstacles`, `get_obstacle`
- `list_roadmaps`, `get_roadmap`
- `get_anonymized_analytics`
- `get_session`, `get_session_summary`

### WRITE Tools (Requires WRITE permission & token):
- `create_question`, `update_question`
- `create_content_draft`

---

## 8. Privacy & Security

- **No PII Collected:** ไม่มีการเก็บชื่อจริง, อีเมล, เบอร์โทรศัพท์ หรือโรงเรียนโดย default
- **Anonymous Session:** ใช้ UUID ที่สร้างขึ้นในเครื่องผู้ใช้
- **Clear My Journey:** ผู้ใช้สามารถลบข้อมูล session ทั้งหมดจากทั้ง LocalStorage และ Cloud ได้ทันที
- **Local-First Reliability:** ทำงานได้เต็มประสิทธิภาพแม้ระบบหลังบ้านออฟไลน์
