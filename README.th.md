[EN](README.md) | [TH](README.th.md)

<h1 align="center">Project Prompt Builder</h1>

<p align="center">
  เลือกสแต็กโปรเจกต์แบบเห็นภาพ แล้วสร้างพรอมป์ตสำหรับ AI coding agent ที่พร้อมคัดลอกไปวาง
</p>

<p align="center">
  <a href="#ภาพรวม">ภาพรวม</a> ·
  <a href="#ฟีเจอร์">ฟีเจอร์</a> ·
  <a href="#เทคโนโลยีที่ใช้">เทคโนโลยี</a> ·
  <a href="#โครงสร้างโปรเจกต์">โครงสร้าง</a> ·
  <a href="#เริ่มต้นใช้งาน">เริ่มต้นใช้งาน</a> ·
  <a href="#ตัวแปรสภาพแวดล้อม">ตัวแปรสภาพแวดล้อม</a> ·
  <a href="#วิธีใช้งาน">วิธีใช้งาน</a> ·
  <a href="#หมายเหตุ">หมายเหตุ</a>
</p>

---

## ภาพรวม

Project Prompt Builder (PPB) คือเว็บแอปหน้าเดียวที่ทำงานฝั่งไคลเอนต์ทั้งหมด เปลี่ยนตัวเลือกเทคโนโลยีและโน้ตโปรเจกต์คร่าว ๆ ให้เป็นบรีฟ Markdown ที่เป็นระบบและให้ผลลัพธ์เดิมเสมอ (deterministic) พร้อมวางลงใน AI coding agent อย่าง Claude Code, Cursor, GitHub Copilot หรือ Codex

ขั้นตอนการใช้งานคือ **เลือก → ตั้งค่า → สร้าง → คัดลอก** ตัวจัดสแต็กแบบมีโครงสร้างส่งข้อมูลให้ engine กฎที่คอยเตือนชุดเทคโนโลยีที่เข้ากันไม่ดี และ pipeline แบบเทมเพลตที่ให้พรอมป์ตเดิมทุกครั้งเมื่ออินพุตเดิม

ตัวแอป **ไม่มี LLM ไม่มี backend และไม่มีฐานข้อมูล** ทุกอย่างทำงานในเบราว์เซอร์และบันทึกใน `localStorage`

## ฟีเจอร์

- **21 หมวดเทคโนโลยี** — แบบเลือกได้ตัวเดียว 10 หมวด และแบบเลือกได้หลายตัว 11 หมวด พร้อมค้นหาจากชื่อและคำอธิบาย
- **12 พรีเซ็ตที่มีชื่อ** — บวกตัวเลือกเริ่มจากศูนย์ การใช้พรีเซ็ตจะแทนที่หมวดที่ระบุไว้ และแก้ไขต่อได้ทั้งหมด
- **7 ช่องข้อมูลความต้องการแบบข้อความธรรมชาติ** — ชื่อโปรเจกต์, คำอธิบาย, กลุ่มผู้ใช้เป้าหมาย, ฟีเจอร์หลัก, ความต้องการทางธุรกิจ, ความต้องการทางเทคนิค และคำสั่งเพิ่มเติม
- **3 โหมดพรอมป์ต** — Basic, Standard และ Production
- **ตัวอย่างพรอมป์ตแบบสด** — จำนวนคำ จำนวนตัวอักษร จำนวนข้อทักท้วง และป้าย **Edited** หลังแก้ไขด้วยมือ
- **คำเตือนเรื่องความเข้ากันได้แบบไม่บล็อก** — อธิบายปัญหาและลิงก์ไปยังหมวดที่เกี่ยวข้อง ไม่มีอะไรบล็อกการใช้งาน ยกเว้นเพดานเลือกฐานข้อมูลได้ไม่เกิน 2 รายการ
- **แก้ไขด้วยมือ** — แก้พรอมป์ตที่สร้างในหน้าเว็บ บันทึกไว้ และย้อนกลับเป็นเวอร์ชันที่สร้างอัตโนมัติได้ทุกเมื่อ
- **คัดลอกไปยังคลิปบอร์ด** — พร้อมกลไกสำรองผ่าน textarea ซ่อนไว้สำหรับสภาพแวดล้อมที่จำกัดสิทธิ์
- **ลิงก์แชร์** — การเปิด URL ที่มีพารามิเตอร์ `?c=` (JSON แบบ base64url) จะนำการตั้งค่าทั้งชุดมาใช้ โดยชนะสถานะที่บันทึกไว้ การเข้ารหัสมีอยู่ใน `lib/share/codec.ts` แต่ยังไม่มีปุ่มใน UI สำหรับสร้างลิงก์
- **ธีมมืด / สว่าง** — ตรวจจับค่าจากระบบปฏิบัติการและจำค่าไว้ใน localStorage
- **เลย์เอาต์ตอบสนอง** — บนเดสก์ท็อปแสดงสองบานหน้าต่างเคียงกัน บนมือถือสลับด้วยแท็บพร้อมแถบคัดลอกแบบติดขอบล่าง
- **บันทึกอัตโนมัติ** — การเลือก ความต้องการ โหมด และ custom skills อยู่รอดหลังรีเฟรช
- **SEO/GEO** — metadata, การ์ด Open Graph และ Twitter, JSON-LD ชนิด `SoftwareApplication`, `sitemap.xml`, `robots.txt` และ `public/llms.txt`

### โหมดพรอมป์ต

| โหมด | เนื้อหา |
| --- | --- |
| **Basic** | บทบาท ภาพรวมโปรเจกต์ สแต็กเทคโนโลยีพร้อมโน้ตการเชื่อมต่อ ความต้องการ และสิ่งที่ต้องส่งมอบ |
| **Standard** | เพิ่มคำแนะนำด้านสถาปัตยกรรม agent skills กฎการพัฒนา คุณภาพโค้ด การทดสอบ และการ deploy |
| **Production** | เพิ่มความปลอดภัย การตรวจสอบข้อมูล/จัดการข้อผิดพลาด การบันทึก log/observability, CI/CD และประสิทธิภาพ/การขยายระบบ พร้อมบล็อกคำสั่งสำหรับ AI agent ปิดท้าย |

## เทคโนโลยีที่ใช้

| ส่วน | ตัวเลือก |
| --- | --- |
| เฟรมเวิร์ก | [Next.js 16.4.0](https://nextjs.org) (App Router, Turbopack) · React 19.3.0 |
| ภาษา | TypeScript 5 (strict, `noEmit`) |
| สไตล์ | [Tailwind CSS](https://tailwindcss.com) v4 ผ่าน `@tailwindcss/turbopack` |
| State | [Zustand](https://zustand-demo.pmnd.rs) 5 พร้อม middleware `persist` |
| ไอคอน | [`lucide-react`](https://lucide.dev) |
| เครื่องมือ UI | `class-variance-authority`, `clsx`, `tailwind-merge` (helper `cn`) |
| การทดสอบ | [Vitest](https://vitest.dev) 5 + Testing Library + jsdom |
| Linting | ESLint 9 ร่วมกับ `eslint-config-next` |
| ตัวจัดการแพ็กเกจ | npm (`package-lock.json`) |

## โครงสร้างโปรเจกต์

```
app/                    โครงหลัก Next.js App Router: layout, page, robots, sitemap, globals.css
components/
  configurator/         แถบพรีเซ็ต, หมวดหมู่, การ์ดตัวเลือก, ฟอร์มความต้องการ, custom skills
  prompt/               สวิตช์โหมด, รายการคำเตือน, ตัวอย่างพรอมป์ต
  ui/                   primitive: Button, Card, ConfirmDialog
  shared-config-loader.tsx
  theme-toggle.tsx
lib/
  data/                 catalog.ts (หมวด/ตัวเลือก), presets.ts, logos.ts
  store/useConfig.ts    Zustand configuration ที่บันทึกอัตโนมัติ
  prompt/               builder.ts, stack-intel.ts, useGeneratedPrompt.ts
  compat/rules.ts       engine กฎความเข้ากันได้แบบเตือนเท่านั้น
  share/codec.ts        เข้ารหัส/ถอดรหัส base64url + sanitizer สำหรับ `?c=`
  export/clipboard.ts   คัดลอกคลิปบอร์ดพร้อมกลไกสำรอง
  domain.ts selections.ts site.ts utils.ts
docs/
  adr/                  บันทึกการตัดสินใจเชิงสถาปัตยกรรม (ADR 0001–0004)
  agents/               เอกสาร issue-tracker, triage-label และ domain
public/
  logos/                ไฟล์ SVG โลโก้แบรนด์
  llms.txt              คำแนะนำสำหรับผู้ช่วย AI
scripts/fetch-logos.mjs ดึงโลโก้แบรนด์จาก svgl.app มาเก็บใหม่
.scratch/               issue tracker แบบ markdown ในเครื่อง (spec และ ticket)
CONTEXT.md              คำศัพท์ของโปรเจกต์ที่ล็อกไว้
AGENTS.md               คำสั่งสำหรับ agent ในโปรเจกต์นี้
```

## เริ่มต้นใช้งาน

ความต้องการ: **Node.js >=22.23.4** (`.nvmrc` กำหนด 24.22.1) และ npm

```bash
npm install
npm run dev
```

เปิด http://localhost:3000 ไม่ต้องมีฐานข้อมูลหรือบริการภายนอก

### คำสั่งสคริปต์

| คำสั่ง | คำอธิบาย |
| --- | --- |
| `npm run dev` | เปิด development server พร้อม Turbopack |
| `npm run build` | build สำหรับ production |
| `npm run start` | เสิร์ฟผลลัพธ์จาก production build |
| `npm run test` | ชุดทดสอบ Vitest (unit + component) |
| `npm run lint` | ESLint |
| `npm run typecheck` | `tsc --noEmit` |
| `npm run fetch-logos` | ดึงโลโก้เทคโนโลยีจาก svgl.app มาเก็บใหม่ |

## ตัวแปรสภาพแวดล้อม

| ชื่อ | จำเป็น | หมายเหตุ |
| --- | --- | --- |
| `NEXT_PUBLIC_SITE_URL` | ไม่บังคับ | canonical site URL ที่ใช้กับ metadata, sitemap และลิงก์แชร์ ถ้าไม่ตั้งค่าจะใช้ `https://project-prompt-builder.vercel.app` |

ดูเทมเพลตได้ที่ `.env.example` ห้ามคอมมิตค่าความลับจริงลงในโปรเจกต์

## วิธีใช้งาน

1. **เลือกพรีเซ็ต** — เลือกหนึ่งใน 12 พรีเซ็ตที่มีชื่อ หรือเริ่มจากตัวเลือก blank/custom
2. **เลือกเทคโนโลยี** — หมวดแบบเลือกตัวเดียวทำงานเหมือนปุ่ม radio ส่วนหมวดแบบหลายตัวกดสลับได้ หมวดฐานข้อมูลจำกัดไม่เกิน 2 ตัวเลือก และการเลือกตัวที่สองจะถูกเตือนว่าเป็นสถาปัตยกรรมหลายฐานข้อมูล
3. **กรอกความต้องการ** — กรอกทั้งเจ็ดช่อง ช่อง custom agent skills พิมพ์บรรทัดละหนึ่งรายการ และเครื่องหมายสแลชนำหน้าจะถูกตัดออก
4. **เลือกโหมด** — Basic, Standard หรือ Production ควบคุมความละเอียดของพรอมป์ตที่สร้าง
5. **ตรวจสอบ** — ตัวอย่างสร้างใหม่แบบ deterministic ทุกครั้งที่เปลี่ยนแปลง คำเตือนความเข้ากันได้อธิบายปัญหาและพาไปยังหมวดที่เกี่ยวข้องได้
6. **คัดลอกหรือแก้ไข** — คัดลอกพรอมป์ต หรือแก้ไขด้วยมือแล้วบันทึก **Reset edits** กลับไปเวอร์ชันที่สร้างอัตโนมัติ และ **Clear** จะมีกล่องยืนยัน
7. **แชร์** — ลิงก์ `?c=` จะนำการตั้งค่าทั้งชุดมาใช้เมื่อเปิด ปัจจุบันยังไม่มีตัวควบคุมใน UI สำหรับสร้างลิงก์

## หมายเหตุ

- **การสร้างแบบ deterministic** — `buildPrompt(config)` ใน `lib/prompt/builder.ts` ทำงานตามลำดับ normalize → stack-intel notes → สร้างแต่ละหัวข้อ → ประกอบตามโหมด และไม่เรียก LLM
- **การตรวจสอบแบบเตือนเท่านั้น** — `evaluateRules(selections)` ใน `lib/compat/rules.ts` คืนค่า `ValidationIssue[]` สำหรับชุดเช่น `sqlc` กับฐานข้อมูลที่ไม่ใช่ SQL, Vercel กับ backend ที่ไม่ใช่ Node หรือ tRPC ที่ไม่มี backend เป็น Node
- **คีย์ที่เก็บ** — การตั้งค่าใช้ `ppb-config-v1`; ธีมใช้ `ppb-theme`
- **คำศัพท์ของโดเมน** — กำหนดไว้ใน [CONTEXT.md](CONTEXT.md) และการตัดสินใจด้านสถาปัตยกรรมอยู่ใน [docs/adr/](docs/adr/) (0001–0004)
- **การติดตามงาน** — ใช้ markdown tracker ในเครื่องใต้ `.scratch/` ดูรายละเอียดที่ [docs/agents/](docs/agents/)
- **การทดสอบ** — Vitest และ Testing Library รันใน jsdom พร้อม `@testing-library/jest-dom` ไฟล์ทดสอบอยู่ข้างโค้ดที่ทดสอบ (`*.test.ts` / `*.test.tsx`)
- **โลโก้** — ดึงมาจาก [svgl.app](https://svgl.app) รีเฟรชได้ด้วย `npm run fetch-logos` ตัวเลือกที่ไม่มีโลโก้จะแสดงเป็นแท่งตัวอักษร
- **การ deploy** — deploy ขึ้น Vercel ได้ทันที ตั้ง `NEXT_PUBLIC_SITE_URL` เป็น URL production ที่เป็น canonical เพื่อให้ metadata และ sitemap ถูกต้อง
- **สัญญาอนุญาต** — MIT (ดู [LICENSE](LICENSE))
