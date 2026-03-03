Hey listen up. Here's what I want. Based on this chat conversation. I want a clear folder of each steps to take, containing markdown only, and I only want a file to clearly state guide of using svelte 5, and not svelte 4, guide saying to use scss, and no tailwind to use @lucide/svelte instead of lucide/svelte as @lucide/svelte is for svelte 5, guide saying to well organize nested scss, and to not use underscore in scss naming scheme, and to use a scss this name convention

    .folder-li .tree-list {

        list-style: none;

        margin: 0;

        padding: 0;

        display: grid;

        gap: var(--tree-view-gap, 2px);

    }

  

    /* Shared row frame (without indent) */

    .folder-row,

    .file-item {

        display: flex;

        align-items: center;

        gap: 6px;

        height: var(--row-height);

        padding: 0 8px; /* base horizontal padding; left will be overridden below */

        border-bottom: 1px solid var(--border-color);

        cursor: default;

        position: relative;

  

        &:hover {

            background: var(--hover-bg);

        }

  

        &.active {

            background: var(--active-bg);

        }

    }

  

    /* Fill gap between adjacent selected items using pseudo-element */

    .file-item.active + .file-item.active::before,

    .file-item.active + .folder-li:has(> .folder > .folder-row.active) > .folder > .folder-row::before,

    .folder-li:has(> .folder > .folder-row.active) + .file-item.active::before,

    .folder-li:has(> .folder > .folder-row.active)

        + .folder-li:has(> .folder > .folder-row.active)

        > .folder

        > .folder-row::before {

        content: '';

        position: absolute;

        top: calc(-1 * var(--tree-view-gap, 2px));

        left: 0;

        right: 0;

        height: var(--tree-view-gap, 2px);

        background: var(--active-bg);

        pointer-events: none;

    }
    
   Where we don't have two dash right next to each other, no double dashes.
   
   Out of all the files you create, make sure you do some thourough research on how to use sveltekit properly, especially in terms of structuring files, and capability, as it is rich.



ChatGPT Conversation:

The app that I want to make is supposed to help manage lottery, make able to buy lotto, haitian borllete, and I need to know everything will be needed, and I'd like you to formulate a bunch of questions to help me organize my thought and come up with a perfect plan and prompt. I want a perfect plan that contain everything that I will need along with every steps, helping everything come out neat, and beautifull, making things robust yet not over using npm packages. I need the plan to properly make use of svelte, prisma, tauri, etc and to also have all the schema that I will be needing in prisma, to contain everything I will need to make, as a developer. Your job is todo the thinking, organization, architecture, and planning for me, and I will be writing the backend code and the frontend code. Make sure in the front-end code you've well thought out how to keep things simple, neat, ui/ux friendly, so that any person any age can understand how the app work. Make sure you plan out every page, and every components. For the backend make sure you've plan out how to keep things straight forward and neat. For the packages I will be using in npm, make sure you've used the best packages, make sure everything is safe, and encryption safe, and since where working with money, make sure to use the best practices, the best packages. also make sure we use best practice in svelte, prisma, database, tauri. These or my draft thoughts: This app needs to work for mobile, desktop, web, and app, will be using tauri for the app. I need to know the pages I will need, everything I will need, the best packages, will be using prisma, tauri, sveltekit, and I want to use the best practices, and to completely make use of sveltekit server and make use of https://svelte.dev/docs/kit/remote-functions. What I want from the ui: I need beautiful, professional, and I prefer usability, seemless, non overwhelm over professional. Basically I want something that the user will use and quickly adapt and understand everything that's going on, over something that is complex, meaning everything is simple and easy no navigate well thought off, and organized. App sections I'm looking for and pages. Basically this app will be for, consumer, and up the chain, bas I'm thinking the app parts, and component some of which needs to be reusable so that everything is familiar accross different user, and permission type, and accross different vendors and companies. Basically I'm thinking we'll need page path for client, vendors, independent companies, app maintainer and administrators. For vendors, we may have staff, management etc, as a vendor could be a single person, or have a staff and a manager and or assistant manager, and different level of permission. I'm unsure how we could make these done very neatly. Will likely also need settings pages, tickets, results, reports, etc. I'm thinking we have a table that rows could expand to show more details etc 1. My app helps solves the lottery issue that's going on in Haiti, it helps manage all lottery in haiti, It's supposed to help sell, manage, and be trustworty. 2. Both 3. Both, and I'm also looking for to be able to sell online, and have tickets sent to phone number or email, and in person, ability to print out ticket, or buy online in person. 4. Everything must be in present in initial release 5. Haiti 6. Yes 7. Not initialily 8. No 9. Unsure, if legall no geofencing 10. Yes online, and offline, with vendors being able to opt in for online 11. Unsure 12. All haitian borllet and gambelling games, including Mariage, Lotto 3, Lotto 4, Lotto 5 13. I'm thinking something like this for some of the games Mariage: If both chosen numbers appear among the three winning borlette numbers, the payout is 1000× the bets. This high multiplier reflects the long odds of roughly 1 in 5,000 for a mariage wins. (E.g., a 5 HTG bet would win 5,000 HTG if successful.) Lotto 3: If the 3-digit number matches the Pick 3 exactly, default payout is 1000× the bet (similar to Mariage). For example, a 1 HTG bet on an exact 3-digit wins 1000 HTG. This is in line with typical Pick 3 lottery payouts, slightly adjusted for local conditions (in many lotteries a $1 bet yields $500, which is 500×, but Haitian private lotteries traditionally offer higher multiples to attract playersss). Lotto 4: For a correct 4-digit match, the default payout is 50,000× the bet. Whether the player chose Option 1, 2, or 3, if their 4-digit guess aligns with the resultant 4-digit, they win 50,000 times their stake. For example, a 1 HTG bet would win 50,000 HTG. (By comparison, in state lotteries a $1 Pick 4 straight win pays $5,000, i.e. 5000×, but the borlette system often advertises larger multipliers to outdo official lotteries – 50,000× here is substantially higher, giving the house a smaller margin on this game.) Lotto 5: For an exact 5-digit match under any of the three options, the default payout is 100,000× the bet. This means a 1 HTG bet could win 100,000 HTG if all five digits are correct. Such a high reward corresponds to the very low probability of winning (1 in 100,000 for an exact match). It essentially functions like a jackpot-tier game for the borlette system. 14. Unsure 15. Unsure 16. Not yet 30 . Yes 31. Yes

Thought for 27m 16s

Below is a complete blueprint you can build from. It includes:

- A **question set** to finalize requirements (so you don’t miss anything)
    
- A **clean multi-tenant architecture** (consumer + vendors + operators + platform admins)
    
- A **page-by-page** plan (routes, layouts, nav, UX)
    
- A **component-by-component** plan (reusable building blocks)
    
- A **backend plan** that stays straightforward
    
- A **Prisma schema** that covers the whole system (you can trim later)
    
- A **minimal-but-strong npm package list** (security-first, not “npm soup”)
    
- A **step-by-step implementation roadmap**
    
- A **ready-to-use “project prompt”** you can reuse for coding tasks
    

I’m going to assume you want a **real-money, production-grade** system (auditability, fraud resistance, offline/online, and reconciliation).

---

## Reality check: what makes this hard

This isn’t “a lottery UI” — it’s a **regulated financial system** with:

- high fraud incentives (counterfeit tickets, result tampering, staff manipulation)
    
- cash handling and reconciliation
    
- payouts and liability management
    
- online payments + chargebacks
    
- offline sales (hardest part)
    

So the plan below treats it like a **payments + ticketing + settlement platform**.

---

## Key decisions up front (so the plan stays coherent)

### 1) One platform, multiple business layers

You described: consumers, vendors, independent companies, and platform administrators.  
That maps cleanly to a **multi-tenant model**:

- **Platform** (you / maintainers)
    
- **Operators** (independent companies that define games/payouts, publish results, settle)
    
- **Vendors** (shops/agents that sell tickets, opt-in for online, pay/receive balances)
    
- **Consumers** (buy online, keep ticket history, claim winnings)
    

### 2) Remote functions are great, but they’re experimental

SvelteKit remote functions:

- are **experimental** and require opt-in `kit.experimental.remoteFunctions`
    
- run **always on the server** and become client-side `fetch` wrappers to generated endpoints
    
- must validate inputs because they expose endpoints; Svelte docs recommend Standard Schema libs like **Zod or Valibot**
    
- you can harden bad input responses with `handleValidationError` to avoid helping attackers
    
- and **you must not use event.url/params/route for authorization decisions** inside remote functions
    

We’ll use remote functions heavily, but we’ll design so you can fall back to endpoints if needed.

### 3) Tauri v2 is the right direction for desktop + mobile

Tauri v2 is stable and supports Linux/macOS/Windows **and** Android/iOS.  
It also has strong security primitives, and for secret storage you can use the **Stronghold** plugin with capability permissions.

---

## Questions to finalize your “perfect plan”

Answering these will remove 80% of surprises.

### A. Legal/compliance (Haiti)

1. Are you operating as a **platform provider** or the **lottery operator** (the entity responsible for paying winnings)?
    
2. Minimum age requirement (18+?) and do you need **age verification** at sign-up or at payout time?
    
3. Do you need **KYC** for payouts above a threshold?
    
4. What tax/fees apply to winnings or vendor commissions?
    
5. Do you need a dispute policy (ticket lost, wrong number entered, result correction)?
    

### B. Business model

6. How does money flow?
    
    - consumer → platform → vendor? or consumer → vendor → operator?
        
7. Do vendors have a **wallet/balance** with the operator (top-up / settlement), or is everything cash-based?
    
8. Vendor commission:
    
    - percent of sales?
        
    - percent of net?
        
    - fixed per ticket?
        
9. Do you support **credit** (vendor sells without immediate payment to operator), or only with balance?
    

### C. Games & draws

10. Are winning numbers sourced from:
    
    - your own draws
        
    - Haitian providers
        
    - US draws (e.g., NY midday/evening), etc?
        
11. How many draws per day? Exact times? (timezone: America/Port-au-Prince)
    
12. Cutoff rules: how many minutes before draw does ticket sales close?
    
13. Do payout multipliers vary by:
    
    - operator
        
    - vendor
        
    - game
        
    - time/draw
        
14. Risk limits:
    
    - max bet per ticket?
        
    - max payout per ticket?
        
    - max exposure per number/pick per draw?
        

### D. Online sales & delivery

15. Are consumers required to create accounts, or can they buy as guest with phone/email?
    
16. Ticket delivery:
    
    - SMS only, email only, both?
        
17. Must online tickets be linked to a specific vendor (vendor opt-in), or can platform fulfill?
    

### E. Payouts & claims

18. Where can winnings be claimed?
    
    - any vendor?
        
    - only the selling vendor?
        
    - operator offices?
        
19. Payout methods:
    
    - cash at vendor
        
    - mobile money transfer
        
    - store credit
        
20. Do you need “partial payouts” (vendor pays small wins, operator pays big wins)?
    

### F. Offline mode

21. What does “offline” mean operationally?
    
    - No internet, but vendor still must print “valid” tickets?
        
22. Do you need offline tickets to be **guaranteed valid** (no later server rejection), or can they be “pending confirmation”?
    
23. Should offline mode support:
    
    - selling
        
    - payout/claim
        
    - only ticket lookup?
        

### G. Operations & support

24. Who can correct mistakes (wrong entry, cancellation)?
    
25. Do you need refunds/cancellations? Under what timeframe?
    
26. Do you need a support inbox/ticket system?
    

### H. UX

27. Languages: Haitian Creole + French + English?
    
28. Accessibility: large text mode, “simple mode” for elderly?
    
29. Do vendors require a “super fast” sales mode (like a POS keypad)?
    

---

## Target UX principles (to match “simple, seamless, non-overwhelming”)

These are design constraints for every page:

1. **One primary action per screen** (especially vendor flow).
    
2. **No dense dashboards first**. Home pages show _today’s essentials only_.
    
3. **Progressive disclosure**: advanced controls hidden behind “More”.
    
4. **Consistent component patterns** across roles (same table, same ticket card, same status badges).
    
5. **Large tap targets** and numeric keypad input for bets.
    
6. **Receipts are always verifiable** (QR + ticket code).
    

---

## Information architecture: route groups + navigation

### Route groups (SvelteKit)

- `(public)` – marketing, login, public results
    
- `(consumer)` – consumer app
    
- `(vendor)` – point-of-sale + claims
    
- `(operator)` – company admin (games, draws, vendors, settlement)
    
- `(platform)` – platform maintainer/admin
    

Each group gets:

- its own `+layout.server.ts` gate (auth/roles)
    
- its own `+layout.svelte` shell (nav style + responsiveness)
    

---

## Pages you will need (every role)

### 1) Public (no login)

**Routes**

- `/` – landing (simple, “Buy ticket”, “Check results”, “Vendor login”)
    
- `/results` – results browser (date + draw + winning numbers)
    
- `/ticket/verify` – verify by ticket code / QR
    
- `/login` – login
    
- `/register` – consumer registration
    
- `/vendor/login` – vendor/staff login (separate branding helps)
    
- `/terms`, `/privacy`, `/responsible-gaming`
    

**Key UI**  
Public results + ticket verify builds trust immediately.

---

### 2) Consumer app

**Navigation:** bottom bar (mobile-first)

- Home
    
- Buy
    
- Tickets
    
- Results
    
- Account
    

**Routes**

- `/app` – Home
    
    - “Buy ticket” big button
        
    - “My active tickets”
        
    - “Latest results”
        
- `/app/buy` – Buy flow entry (choose vendor if required)
    
- `/app/buy/[drawId]` – Ticket builder wizard
    
- `/app/checkout/[ticketId]` – Payment + confirm
    
- `/app/tickets` – Ticket list (filters: active/won/lost/pending)
    
- `/app/tickets/[ticketId]` – Ticket detail + QR + share
    
- `/app/results` – results browser
    
- `/app/account`
    
    - profile
        
    - security
        
    - notifications
        
    - language
        

---

### 3) Vendor app (POS)

**Navigation:** “POS style” (big buttons, few items)

- Sell
    
- Tickets
    
- Claims
    
- Sync
    
- Reports
    
- Settings
    

**Routes**

- `/vendor` – Dashboard
    
    - today’s sales
        
    - pending offline sync count
        
    - next draw cutoff countdown
        
- `/vendor/sell` – fast sell (default screen)
    
- `/vendor/sell/[drawId]` – ticket builder (optimized)
    
- `/vendor/tickets`
    
    - search by code / scan QR
        
    - reprint
        
    - cancel (role-based)
        
- `/vendor/tickets/[ticketId]`
    
- `/vendor/claims`
    
    - scan ticket
        
    - show win breakdown
        
    - pay out + receipt
        
- `/vendor/sync`
    
    - offline queue
        
    - last sync time
        
    - “force sync”
        
- `/vendor/reports`
    
    - daily totals
        
    - by draw
        
    - payouts
        
    - commissions
        
- `/vendor/settings`
    
    - printer
        
    - staff (manager only)
        
    - online opt-in toggle (if allowed)
        
    - device/terminal status
        

---

### 4) Operator admin (independent company)

**Navigation:** left sidebar (desktop admin)

- Overview
    
- Vendors
    
- Games
    
- Draws
    
- Results
    
- Settlement
    
- Finance
    
- Users
    
- Audit
    

**Routes**

- `/operator` – Overview
    
- `/operator/vendors`
    
    - list + onboarding + contracts
        
- `/operator/vendors/[vendorOrgId]`
    
    - contract: commission, limits, online enabled
        
    - performance
        
- `/operator/games`
    
    - define games + payout rules + validations
        
- `/operator/games/[gameId]`
    
- `/operator/draws`
    
    - draw schedules
        
    - upcoming draws
        
- `/operator/draws/[drawId]`
    
- `/operator/results`
    
    - “enter results” + history
        
- `/operator/settlement`
    
    - run settlement per draw
        
    - vendor balances
        
- `/operator/finance`
    
    - deposits, payouts, adjustments
        
- `/operator/users`
    
    - staff roles
        
- `/operator/audit`
    
    - immutable log viewer
        

---

### 5) Platform admin (maintainers)

**Routes**

- `/platform` – health overview
    
- `/platform/operators`
    
- `/platform/operators/[operatorOrgId]`
    
- `/platform/users` – platform-level user search
    
- `/platform/audit` – global audit log
    
- `/platform/config` – feature flags, global settings
    

---

## Reusable component system (simple, consistent)

Build a small internal UI kit. Don’t adopt a massive UI framework unless you must.

### Core UI components (used everywhere)

- `AppShell` (layout container)
    
- `TopBar`, `SideBar`, `BottomNav`
    
- `Card`, `SectionHeader`
    
- `Button`, `IconButton`
    
- `Input`, `Select`, `Textarea`
    
- `NumberInput` + `Keypad` (vendor + consumer)
    
- `Badge` (status)
    
- `Toast` / `InlineAlert`
    
- `Modal` + `Drawer` (mobile)
    
- `Tabs` (reports, ticket detail)
    
- `DataTable`
    
    - sorting
        
    - expandable row
        
    - row actions
        
- `EmptyState`
    
- `SkeletonLoading`
    
- `PermissionGate` (show/hide actions based on role)
    

### Domain components

**Ticketing**

- `GameCard`
    
- `DrawCard` (with cutoff countdown)
    
- `TicketBuilderWizard`
    
- `PickInput`
    
    - lotto3/4/5 digit input
        
    - mariage pair input
        
- `TicketLineEditor`
    
- `TicketSummary`
    
- `TicketReceipt` (print-friendly)
    
- `QRCodeBlock`
    

**Claims**

- `TicketScanner` (camera + manual code input)
    
- `WinningBreakdown`
    
- `PayoutConfirmModal`
    

**Admin**

- `ContractEditor` (commission/limits)
    
- `PayoutRuleEditor`
    
- `DrawScheduleEditor`
    
- `ResultEntryForm`
    
- `SettlementPreviewTable`
    

---

## Backend architecture (straightforward, neat)

### “Clean but not over-engineered” layering

1. **Route layer** (SvelteKit pages + layouts)
    
2. **Remote functions** for reads/writes
    
3. **Service layer** (pure functions) in `src/lib/server/services/*`
    
4. **Data access** via Prisma in `src/lib/server/db.ts`
    
5. **Policies** for auth/permissions in `src/lib/server/policy/*`
    

### Why remote functions fit well here

Remote functions give type-safe server calls and can access Prisma safely server-side.  
They are transformed into fetch wrappers client-side, so treat them like public endpoints: validate inputs (Zod/Valibot) and harden validation errors with `handleValidationError`.

### Auth approach

SvelteKit itself encourages session/cookie patterns and discusses sessions vs JWT tradeoffs.  
For a money app, prefer **server-stored sessions** for immediate revocation.

Implement:

- `hooks.server.ts` loads session from cookie and sets `locals.user`
    
- role/membership checks in a policy helper
    
- re-auth (password/2FA) for sensitive actions (settlement, manual adjustments)
    

Cookie/session best practices: use secure cookie attributes and avoid session IDs in URLs (OWASP).

---

## Tauri plan (desktop + mobile + secure storage)

### What Tauri does for you

- Wrap the same UI for desktop/mobile.
    
- Provide native features: secure storage, printing integration, camera scanning.
    
- Stronghold plugin can store secrets securely and uses capability permissions.
    

### Secure local storage strategy

- Store only what’s needed locally:
    
    - offline queue items
        
    - user auth token/session key (Tauri secure store)
        
    - printer config
        

Use:

- **Stronghold** for secrets (session token / offline signing keys)
    
- IndexedDB for offline queue data (optionally encrypt payloads using WebCrypto and keep key in Stronghold)
    

---

## Offline mode design (robust, trust-preserving)

Offline lottery sales are where most systems break. Here’s the cleanest approach:

### Option A (simpler): “Pending confirmation”

- Vendor can create ticket offline, print a receipt that says **PENDING**
    
- When online, ticket is submitted; server either confirms or rejects (if cutoff passed)
    
- If rejected, vendor refunds
    

Pros: easiest  
Cons: trust hit (“pending” tickets)

### Option B (recommended): Offline ticket credits (pre-authorized)

- When online, terminal requests an **Offline Batch** for upcoming draws (pre-authorized ticket IDs)
    
- Each credit is valid only for:
    
    - a specific draw or draw window
        
    - a specific terminal
        
    - and expires at cutoff
        
- Offline, terminal consumes a credit to create a ticket that is cryptographically verifiable
    
- When syncing later, server verifies the credit and accepts ticket even if submitted after reconnect (as long as created before cutoff)
    

This creates “offline-valid tickets” without letting vendors backdate.

The Prisma schema below includes structures to support this.

---

## Core workflows (end-to-end)

### 1) In-person vendor sale

1. cashier selects draw
    
2. enters picks + amounts
    
3. system checks:
    
    - cutoff not passed
        
    - exposure limits ok
        
    - max payout rules
        
4. ticket created (transaction)
    
5. receipt shown + print
    
6. (optional) send to phone/email
    

### 2) Consumer online sale

1. consumer selects vendor (only those online-enabled)
    
2. builds ticket
    
3. pays online (payment provider)
    
4. server confirms payment → ticket becomes CONFIRMED
    
5. ticket delivered via SMS/email + app “My Tickets”
    

### 3) Results entry + settlement

1. operator enters results
    
2. system locks results version and triggers settlement job
    
3. compute winners and vendor balances
    
4. update ticket lines: win amounts
    
5. vendors see “what’s owed” + claims allowed
    

### 4) Claim payout at vendor

1. scan QR or enter ticket code
    
2. server verifies:
    
    - ticket is confirmed
        
    - not already claimed
        
    - draw settled
        
3. show breakdown
    
4. cashier confirms payout (creates payout record + audit log)
    
5. print payout receipt
    

---

## Minimal npm packages (security-first, not overkill)

You can build most of this with “boring” dependencies:

### Required

- `@sveltejs/kit`, `svelte`
    
- `prisma`, `@prisma/client`
    
- **Validation**: `valibot` (light) or `zod` (popular). Remote functions docs explicitly reference Standard Schema libs like Zod/Valibot.
    
- `argon2` (password hashing; choose argon2id)
    
- `pino` (structured logs) + `pino-pretty` (dev)
    

### Strongly recommended

- `qrcode` (generate QR codes for tickets/receipts)
    
- `nanoid` (only if you don’t want `crypto.randomUUID()`)
    

### Front-end styling

- `tailwindcss` (optional but practical)
    
    - If you want _minimal packages_, you can skip Tailwind and use CSS variables + utility classes you write yourself.
        

### Tauri

- `@tauri-apps/api`
    
- `@tauri-apps/plugin-stronghold` (secure secrets)
    

### Avoid unless truly needed

- heavy UI frameworks
    
- state libraries (you can do this with Svelte stores)
    
- ORMs besides Prisma
    

---

## Prisma schema (baseline, production-minded)

This schema is intentionally “complete”. You can delete what you don’t need later.

`// prisma/schema.prisma  generator client {   provider = "prisma-client-js" }  datasource db {   provider = "postgresql"   url      = env("DATABASE_URL") }  enum OrgKind {   PLATFORM   OPERATOR   VENDOR }  enum UserStatus {   ACTIVE   SUSPENDED   DELETED }  enum MembershipRole {   PLATFORM_ADMIN   PLATFORM_SUPPORT    OPERATOR_ADMIN   OPERATOR_FINANCE   OPERATOR_RESULTS    VENDOR_OWNER   VENDOR_MANAGER   VENDOR_CASHIER    CONSUMER }  enum OutletType {   HQ   STORE   KIOSK   MOBILE_AGENT }  enum TerminalStatus {   ACTIVE   DISABLED }  enum GameKind {   BORLETTE_MARIAGE   LOTTO3   LOTTO4   LOTTO5   OTHER }  enum DrawStatus {   SCHEDULED   OPEN   CLOSED   RESULTED   SETTLED   CANCELLED }  enum TicketChannel {   IN_PERSON   ONLINE }  enum TicketStatus {   PENDING     // created but not yet confirmed (e.g., offline pending)   CONFIRMED   // accepted for draw   CANCELLED   VOIDED }  enum PaymentMethod {   CASH   MOBILE_MONEY   CARD   BANK_TRANSFER   WALLET }  enum PaymentStatus {   PENDING   AUTHORIZED   SETTLED   FAILED   REFUNDED   CHARGEBACK }  enum ClaimStatus {   REQUESTED   APPROVED   PAID   REJECTED   VOIDED }  enum NotificationChannel {   SMS   EMAIL   PUSH }  enum NotificationStatus {   QUEUED   SENT   FAILED }  model User {   id            String     @id @default(uuid())   email         String?    @unique   phone         String?    @unique   displayName   String   passwordHash  String?   status        UserStatus @default(ACTIVE)    createdAt     DateTime   @default(now())   updatedAt     DateTime   @updatedAt   lastLoginAt   DateTime?    memberships   Membership[]   sessions      UserSession[]    // audit relations   createdTickets Ticket[]   @relation("TicketCreatedBy")   enteredResults DrawResult[] @relation("ResultEnteredBy")   auditLogs      AuditLog[] @relation("AuditActor")    @@index([phone])   @@index([email]) }  model UserSession {   id          String   @id @default(uuid())   userId      String   user        User     @relation(fields: [userId], references: [id], onDelete: Cascade)    createdAt   DateTime @default(now())   expiresAt   DateTime   revokedAt   DateTime?    ip          String?   userAgent   String?    @@index([userId, expiresAt]) }  model Org {   id          String   @id @default(uuid())   kind        OrgKind   name        String   parentId    String?   parent      Org?     @relation("OrgParent", fields: [parentId], references: [id])   children    Org[]    @relation("OrgParent")    // optional metadata   email       String?   phone       String?   address     String?   country     String?  @default("HT")   timezone    String?  @default("America/Port-au-Prince")    createdAt   DateTime @default(now())   updatedAt   DateTime @updatedAt    outlets     Outlet[]   memberships Membership[]    // operator domain   games       Game[]   drawTemplates DrawTemplate[]   draws       Draw[]    // vendor/operator contracts   operatorContracts VendorContract[] @relation("OperatorContracts")   vendorContracts    VendorContract[] @relation("VendorContracts")    @@index([kind])   @@index([parentId]) }  model Outlet {   id          String     @id @default(uuid())   orgId       String   org         Org        @relation(fields: [orgId], references: [id], onDelete: Cascade)    type        OutletType @default(STORE)   name        String   address     String?   city        String?   region      String?    isActive    Boolean    @default(true)    createdAt   DateTime   @default(now())   updatedAt   DateTime   @updatedAt    terminals   Terminal[]   memberships Membership[]    tickets     Ticket[]   claimsPaid  Claim[]    @relation("ClaimPaidAtOutlet")    @@index([orgId]) }  model Membership {   id        String          @id @default(uuid())   userId    String   orgId     String   outletId  String?   role      MembershipRole    user      User            @relation(fields: [userId], references: [id], onDelete: Cascade)   org       Org             @relation(fields: [orgId], references: [id], onDelete: Cascade)   outlet    Outlet?         @relation(fields: [outletId], references: [id], onDelete: Cascade)    createdAt DateTime        @default(now())    @@unique([userId, orgId, outletId, role])   @@index([orgId, role])   @@index([outletId, role]) }  model VendorContract {   id            String   @id @default(uuid())   operatorOrgId String   vendorOrgId   String    operator      Org      @relation("OperatorContracts", fields: [operatorOrgId], references: [id], onDelete: Cascade)   vendor        Org      @relation("VendorContracts", fields: [vendorOrgId], references: [id], onDelete: Cascade)    onlineEnabled Boolean  @default(false)    // commission in basis points (e.g., 500 = 5.00%)   commissionBps Int      @default(0)    // optional risk controls   maxTicketStake     BigInt?   maxTicketPayout    BigInt?   maxExposurePerPick BigInt?    createdAt      DateTime @default(now())   updatedAt      DateTime @updatedAt    @@unique([operatorOrgId, vendorOrgId])   @@index([vendorOrgId]) }  model Terminal {   id            String         @id @default(uuid())   outletId      String   outlet        Outlet         @relation(fields: [outletId], references: [id], onDelete: Cascade)    name          String   status        TerminalStatus @default(ACTIVE)    // if you choose the “offline credits” model   devicePublicKey String? // optional: store public key for offline signing   lastSeenAt      DateTime?    createdAt     DateTime @default(now())   updatedAt     DateTime @updatedAt    offlineBatches OfflineBatch[]   tickets        Ticket[]    @@index([outletId]) }  model Game {   id            String   @id @default(uuid())   operatorOrgId String   operator      Org      @relation(fields: [operatorOrgId], references: [id], onDelete: Cascade)    code          String   // stable identifier like "MARIAGE", "LOTTO3"   kind          GameKind @default(OTHER)   name          String   description   String?    // JSON config: digits, allowed picks, etc.   config        Json?    isEnabled     Boolean  @default(true)    options       GameOption[]   payoutRules   PayoutRule[]   ticketLines   TicketLine[]   exposures     Exposure[]    createdAt     DateTime @default(now())   updatedAt     DateTime @updatedAt    @@unique([operatorOrgId, code])   @@index([operatorOrgId, isEnabled]) }  model GameOption {   id        String @id @default(uuid())   gameId    String   game      Game   @relation(fields: [gameId], references: [id], onDelete: Cascade)    code      String // e.g. "OPT1", "OPT2", "OPT3"   name      String   config    Json?    payoutRules PayoutRule[]   ticketLines TicketLine[]    @@unique([gameId, code]) }  model PayoutRule {   id          String   @id @default(uuid())   gameId      String   optionId    String?    game        Game     @relation(fields: [gameId], references: [id], onDelete: Cascade)   option      GameOption? @relation(fields: [optionId], references: [id], onDelete: SetNull)    // multiplier (e.g., 1000x, 50000x)   multiplier  Int    // optional min/max constraints   minStake    BigInt?   maxStake    BigInt?    createdAt   DateTime @default(now())    @@index([gameId]) }  model DrawTemplate {   id            String   @id @default(uuid())   operatorOrgId String   operator      Org      @relation(fields: [operatorOrgId], references: [id], onDelete: Cascade)    code          String   // e.g. "MORNING", "MIDDAY", "EVENING"   name          String   timezone      String   @default("America/Port-au-Prince")    // JSON schedule definition (you generate Draw instances from it)   schedule      Json?    // cutoff offset in minutes (e.g., 10 => close 10 min before)   cutoffMinutes Int      @default(10)    isEnabled     Boolean  @default(true)    draws         Draw[]    createdAt     DateTime @default(now())   updatedAt     DateTime @updatedAt    @@unique([operatorOrgId, code]) }  model Draw {   id            String    @id @default(uuid())   operatorOrgId String   operator      Org       @relation(fields: [operatorOrgId], references: [id], onDelete: Cascade)    templateId    String?   template      DrawTemplate? @relation(fields: [templateId], references: [id], onDelete: SetNull)    name          String   scheduledAt   DateTime   cutOffAt      DateTime   status        DrawStatus @default(SCHEDULED)    // active result pointer (for correction history)   activeResultId String?  @unique   activeResult   DrawResult? @relation("ActiveResult", fields: [activeResultId], references: [id])    results       DrawResult[] @relation("AllResults")    tickets       Ticket[]   exposures     Exposure[]   settlements   Settlement[]    createdAt     DateTime @default(now())   updatedAt     DateTime @updatedAt    @@index([operatorOrgId, scheduledAt])   @@index([status, cutOffAt]) }  model DrawResult {   id           String   @id @default(uuid())   drawId       String   draw         Draw     @relation("AllResults", fields: [drawId], references: [id], onDelete: Cascade)    enteredByUserId String?   enteredBy     User?   @relation("ResultEnteredBy", fields: [enteredByUserId], references: [id], onDelete: SetNull)    // MANUAL, API, etc. keep as string to avoid enum churn   source       String   @default("MANUAL")    // JSON example: { "borlette": ["12","34","56"], "lotto3": "123", ... }   numbers      Json    createdAt    DateTime @default(now())    // backrelation for active result   activeForDraw Draw?   @relation("ActiveResult") }  model Ticket {   id            String       @id @default(uuid())   code          String       @unique // short human code printed on receipt    drawId        String   draw          Draw         @relation(fields: [drawId], references: [id], onDelete: Restrict)    channel       TicketChannel   status        TicketStatus @default(PENDING)    operatorOrgId String   operator      Org          @relation(fields: [operatorOrgId], references: [id], onDelete: Restrict)    // seller = vendor   vendorOrgId   String?   vendor        Org?         @relation(fields: [vendorOrgId], references: [id], onDelete: SetNull)    outletId      String?   outlet        Outlet?      @relation(fields: [outletId], references: [id], onDelete: SetNull)    terminalId    String?   terminal      Terminal?    @relation(fields: [terminalId], references: [id], onDelete: SetNull)    customerUserId String?   customerUser  User?        @relation(fields: [customerUserId], references: [id], onDelete: SetNull)    recipientPhone String?   recipientEmail String?    totalStake     BigInt   totalPotentialPayout BigInt?    createdAt     DateTime @default(now())   confirmedAt   DateTime?   cancelledAt   DateTime?    createdByUserId String?   createdBy    User?      @relation("TicketCreatedBy", fields: [createdByUserId], references: [id], onDelete: SetNull)    lines         TicketLine[]   payment       Payment?   claim         Claim?    @@index([drawId, status])   @@index([vendorOrgId, createdAt])   @@index([customerUserId, createdAt]) }  model TicketLine {   id          String @id @default(uuid())    ticketId    String   ticket      Ticket @relation(fields: [ticketId], references: [id], onDelete: Cascade)    gameId      String   game        Game   @relation(fields: [gameId], references: [id], onDelete: Restrict)    optionId    String?   option      GameOption? @relation(fields: [optionId], references: [id], onDelete: SetNull)    // Store picks as strings to preserve leading zeros   // For mariage: "12-34"; lotto3: "123"; lotto4: "0123"; lotto5: "00123"   pickKey     String   pickData    Json?    stakeAmount BigInt    // snapshot so rule changes don’t affect old tickets   payoutMultiplier Int   potentialPayout  BigInt?    // filled after settlement   winAmount   BigInt?   evaluatedAt DateTime?    @@index([gameId, pickKey])   @@index([ticketId]) }  model Payment {   id          String        @id @default(uuid())   ticketId    String        @unique   ticket      Ticket        @relation(fields: [ticketId], references: [id], onDelete: Cascade)    method      PaymentMethod   status      PaymentStatus @default(PENDING)    amount      BigInt   currency    String        @default("HTG")    provider    String? // e.g. "moncash", "stripe"   providerRef String? // transaction id    createdAt   DateTime      @default(now())   updatedAt   DateTime      @updatedAt    @@index([status, createdAt]) }  model Claim {   id          String      @id @default(uuid())    ticketId    String      @unique   ticket      Ticket      @relation(fields: [ticketId], references: [id], onDelete: Cascade)    status      ClaimStatus @default(REQUESTED)    requestedAt DateTime    @default(now())   approvedAt  DateTime?   paidAt      DateTime?    payoutMethod PaymentMethod?   payoutAmount BigInt?    paidAtOutletId String?   paidAtOutlet Outlet? @relation("ClaimPaidAtOutlet", fields: [paidAtOutletId], references: [id], onDelete: SetNull)    paidByUserId String?   paidByUser   User? @relation(fields: [paidByUserId], references: [id], onDelete: SetNull)    notes       String?    @@index([status, requestedAt]) }  model Exposure {   id            String  @id @default(uuid())    operatorOrgId String   vendorOrgId   String?   drawId        String   gameId        String    draw          Draw    @relation(fields: [drawId], references: [id], onDelete: Cascade)   game          Game    @relation(fields: [gameId], references: [id], onDelete: Cascade)    // key for the pick   pickKey       String    totalStake    BigInt  @default(0)   totalPotentialPayout BigInt @default(0)    updatedAt     DateTime @updatedAt    @@unique([operatorOrgId, vendorOrgId, drawId, gameId, pickKey])   @@index([drawId, gameId]) }  model OfflineBatch {   id          String   @id @default(uuid())   terminalId  String   terminal    Terminal @relation(fields: [terminalId], references: [id], onDelete: Cascade)    drawId      String?   draw        Draw?    @relation(fields: [drawId], references: [id], onDelete: SetNull)    issuedAt    DateTime @default(now())   expiresAt   DateTime    totalCount  Int   usedCount   Int      @default(0)    // optional signature blob if you implement signed credits   signature   String?    @@index([terminalId, expiresAt]) }  model Settlement {   id            String   @id @default(uuid())   drawId        String   operatorOrgId String    draw          Draw     @relation(fields: [drawId], references: [id], onDelete: Cascade)   operator      Org      @relation(fields: [operatorOrgId], references: [id], onDelete: Cascade)    status        String   @default("RUNNING") // RUNNING, DONE, FAILED   startedAt     DateTime @default(now())   finishedAt    DateTime?    triggeredByUserId String?   triggeredBy   User? @relation(fields: [triggeredByUserId], references: [id], onDelete: SetNull)    summary      Json?    vendorLines  SettlementVendor[]    @@index([drawId, operatorOrgId]) }  model SettlementVendor {   id            String   @id @default(uuid())   settlementId  String   vendorOrgId   String    settlement    Settlement @relation(fields: [settlementId], references: [id], onDelete: Cascade)   vendor        Org        @relation(fields: [vendorOrgId], references: [id], onDelete: Cascade)    totalSales    BigInt   totalWinnings BigInt   totalCommission BigInt   netDueToOperator BigInt // positive means vendor owes operator, negative means operator owes vendor    @@unique([settlementId, vendorOrgId]) }  model Notification {   id        String              @id @default(uuid())    channel   NotificationChannel   to        String   template  String   payload   Json?    status    NotificationStatus  @default(QUEUED)    provider  String?   providerRef String?    error     String?    createdAt DateTime @default(now())   sentAt    DateTime?    @@index([status, createdAt]) }  model AuditLog {   id          String   @id @default(uuid())    orgId       String?   org         Org?     @relation(fields: [orgId], references: [id], onDelete: SetNull)    actorUserId String?   actor       User?    @relation("AuditActor", fields: [actorUserId], references: [id], onDelete: SetNull)    action      String   // e.g., "TICKET_CREATE", "RESULT_ENTER"   entityType  String?   entityId    String?   metadata    Json?    ip          String?   userAgent   String?    createdAt   DateTime @default(now())    @@index([orgId, createdAt])   @@index([action, createdAt]) }`

---

## Remote functions structure (how to organize it neatly)

Remote functions:

- must live in `.remote.ts` files and can be anywhere under `src` **except** `src/lib/server`
    
- run only on server and can access Prisma safely
    
- should validate inputs (Zod/Valibot) because they expose endpoints
    
- should use `handleValidationError` to avoid helpful error details
    
- should not use event url/params for authorization decisions
    

**Recommended file organization**

- `src/lib/server/*`
    
    - `db.ts`
        
    - `services/tickets.ts`
        
    - `services/draws.ts`
        
    - `services/claims.ts`
        
    - `services/settlement.ts`
        
    - `policy/requireRole.ts`
        
- `src/lib/remote/*`
    
    - `tickets.remote.ts`
        
    - `draws.remote.ts`
        
    - `claims.remote.ts`
        
    - `admin.remote.ts`
        

This keeps “business logic” separate from “transport”.

---

## Security checklist (money + trust)

This is non-negotiable for launch.

1. **Every critical action creates an AuditLog**:
    
    - ticket created/cancelled
        
    - result entered/changed
        
    - claim paid
        
    - settlement run
        
2. **Session security**:
    
    - secure, HttpOnly cookies (or secure token storage for Tauri)
        
    - short session lifetime for vendor/admin
        
    - re-auth for sensitive actions
        
    - follow OWASP session guidance
        
3. **Permissions are explicit** (role checks, no “if user.admin” scattered everywhere)
    
4. **DB transactions** for ticket creation (ticket + lines + exposure update)
    
5. **Immutable result history** (DrawResult history + activeResult pointer)
    
6. **Rate limiting** on login, ticket verify, public endpoints (do proxy-level if possible)
    
7. **No floats** for currency (always integer BigInt)
    

---

## Step-by-step build roadmap (keeps it neat)

### Phase 1 — Foundation (week 1–2)

- SvelteKit project structure with route groups and layouts
    
- Prisma + Postgres + migrations
    
- Auth + sessions in hooks
    
- Basic admin shell + vendor shell + consumer shell
    

### Phase 2 — Core lottery domain (week 2–4)

- Games + payout rules
    
- Draw templates + draw instances
    
- Ticket create (in-person + online placeholder)
    
- Ticket receipt rendering + QR
    

### Phase 3 — Vendor POS (week 4–6)

- Fast sell flow
    
- Ticket search + reprint
    
- Basic reports (sales by draw)
    
- Basic claims (manual ticket code lookup)
    

### Phase 4 — Results + settlement (week 6–8)

- Results entry UI
    
- Settlement job
    
- Winner evaluation
    
- Claim verification + payout receipt
    

### Phase 5 — Online purchase (week 8–10)

- Consumer buy flow
    
- Payment integration abstraction
    
- SMS/email delivery pipeline
    

### Phase 6 — Offline mode (week 10+)

- Offline queue + sync
    
- Optional offline ticket credits
    

---

## Your “perfect project prompt” (copy/paste)

Use this whenever you want an AI assistant to generate specific code modules with your constraints:

`You are assisting me in building a production-grade Haiti lottery + borlette management platform.  Stack: - SvelteKit (TypeScript) - Prisma (Postgres) - Tauri v2 (desktop + mobile) - Prefer SvelteKit remote functions for server communication (kit.experimental.remoteFunctions) - Minimal npm packages: only security-essential + small utilities - Security-first: audit logs, safe sessions, strict validation, no floats for money  Product requirements: - Roles: consumer, vendor cashier, vendor manager/owner, operator admin, platform admin - Vendors can sell in-person and optionally online - Tickets can be delivered via SMS/email and printed at vendor - Supports borlette games: Mariage, Lotto3, Lotto4, Lotto5 - Results entry and settlement with immutable history - Claims/payouts at vendors with verification - Offline mode for vendors (queue + sync; optionally offline credits)  Constraints: - Keep UX extremely simple: one primary action per screen, large tap targets, consistent components - Use a small internal component library (Button/Input/Table/Modal/etc.) - Backend code must be organized into service modules and policy checks  Task: Generate the code for [MODULE NAME], including: - routes and components needed - remote functions (with Valibot or Zod validation) - Prisma queries in a service module - permission checks and audit logging - tests or test plan`

---

If you want, I can also produce a **concrete sitemap diagram** and a **complete “game rules spec”** for Mariage/Lotto3/4/5 (inputs, validations, normalized pick keys, winner evaluation), so your ticket line storage and settlement logic stay consistent from day one.

Yes please do.

Thought for 18s

Absolutely — here are (1) a concrete sitemap diagram and (2) a complete, implementation-ready game rules spec (inputs, validation, normalization, pick keys, result shapes, winner evaluation, and test cases).

---

## 1) Concrete sitemap diagram

### A. Route tree (SvelteKit groups)

`/ ├─ (public) │  ├─ /                         (landing) │  ├─ /results                  (public results browser) │  ├─ /results/[drawId]          (draw detail) │  ├─ /ticket/verify             (enter code / scan QR) │  ├─ /ticket/verify/[code]      (deep link ticket verify) │  ├─ /login │  ├─ /register │  ├─ /vendor/login │  ├─ /terms │  ├─ /privacy │  └─ /responsible-gaming │ ├─ (consumer)  /app │  ├─ /app                       (home) │  ├─ /app/buy                   (choose draw + vendor if required) │  ├─ /app/buy/[drawId]           (ticket builder wizard) │  ├─ /app/checkout/[ticketId]    (payment + confirm) │  ├─ /app/tickets                (list) │  ├─ /app/tickets/[ticketId]     (detail + QR + share) │  ├─ /app/results                (results browser) │  ├─ /app/results/[drawId]        (draw detail) │  └─ /app/account │     ├─ /app/account/profile │     ├─ /app/account/security │     ├─ /app/account/notifications │     └─ /app/account/language │ ├─ (vendor) /vendor │  ├─ /vendor                     (today snapshot) │  ├─ /vendor/sell                (fast sell hub) │  ├─ /vendor/sell/[drawId]        (POS ticket builder) │  ├─ /vendor/tickets              (search/list + reprint) │  ├─ /vendor/tickets/[ticketId]   (detail) │  ├─ /vendor/claims               (scan/lookup + payout) │  ├─ /vendor/claims/[ticketId]    (claim detail + payout confirm) │  ├─ /vendor/sync                 (offline queue + sync) │  ├─ /vendor/reports              (daily/by draw/payouts) │  └─ /vendor/settings │     ├─ /vendor/settings/printer │     ├─ /vendor/settings/staff │     ├─ /vendor/settings/online │     └─ /vendor/settings/device │ ├─ (operator) /operator │  ├─ /operator                   (overview) │  ├─ /operator/vendors │  ├─ /operator/vendors/[vendorOrgId] │  ├─ /operator/games │  ├─ /operator/games/[gameId] │  ├─ /operator/draw-templates │  ├─ /operator/draws │  ├─ /operator/draws/[drawId] │  ├─ /operator/results │  ├─ /operator/results/[drawId]  (enter/replace results) │  ├─ /operator/settlement │  ├─ /operator/settlement/[drawId] │  ├─ /operator/finance │  ├─ /operator/users │  └─ /operator/audit │ └─ (platform) /platform    ├─ /platform                   (global health)    ├─ /platform/operators    ├─ /platform/operators/[operatorOrgId]    ├─ /platform/users    ├─ /platform/audit    └─ /platform/config`

### B. Mermaid sitemap (nice for docs)

`flowchart TD   A[/Public/] --> B[Landing]   A --> C[Results]   A --> D[Ticket Verify]   A --> E[Login/Register]    F[/Consumer App/] --> F1[Home]   F --> F2[Buy]   F --> F3[Tickets]   F --> F4[Results]   F --> F5[Account]    G[/Vendor POS/] --> G1[Dashboard]   G --> G2[Sell]   G --> G3[Tickets]   G --> G4[Claims]   G --> G5[Sync]   G --> G6[Reports]   G --> G7[Settings]    H[/Operator Admin/] --> H1[Overview]   H --> H2[Vendors]   H --> H3[Games]   H --> H4[Draws]   H --> H5[Results Entry]   H --> H6[Settlement]   H --> H7[Finance]   H --> H8[Users]   H --> H9[Audit]    I[/Platform Admin/] --> I1[Health]   I --> I2[Operators]   I --> I3[Users]   I --> I4[Audit]   I --> I5[Config]`

---

## 2) Complete game rules spec (Mariage, Lotto 3/4/5)

You said “all Haitian borlette gambling games including Mariage, Lotto 3, Lotto 4, Lotto 5” and that Lotto 4/5 have “Option 1/2/3” but details are unsure. So the spec below:

- locks down **canonical storage** and **evaluation mechanics**
    
- keeps options flexible (you can rename or adjust matching rules later)
    
- stays consistent across UI, Prisma, and settlement logic
    

### Shared conventions (for every game)

#### Money

- Store all money as integer **HTG cents** or integer HTG units (choose one and stick to it).
    
- Use `BigInt` in Prisma for `stakeAmount`, `winAmount`, `potentialPayout`.
    

#### Number formatting

- “2-digit” numbers are strings `"00"`–`"99"` (always zero-padded).
    
- “3-digit” numbers `"000"`–`"999"`.
    
- “4-digit” numbers `"0000"`–`"9999"`.
    
- “5-digit” numbers `"00000"`–`"99999"`.
    

#### Normalization function (must be identical everywhere)

- Trim spaces
    
- Remove separators (`-`, spaces) when appropriate
    
- Validate digits-only after cleaning
    
- Left-pad with zeros to required length
    

#### Canonical `pickKey`

`pickKey` is the unique, normalized key used for:

- exposure tracking
    
- de-duplication
    
- deterministic winner evaluation
    

Rules:

- If order matters → `pickKey` is exact normalized digits
    
- If order does NOT matter → `pickKey` is a sorted canonical form
    

#### Result shape (DrawResult.numbers JSON)

Keep results per draw in a single JSON object with keys per game:

`{   "borlette3": ["12","34","56"],   "lotto3": "123",   "lotto4": "0123",   "lotto5": "00123" }`

If a draw doesn’t provide a game, omit it (or set null) and mark game unavailable for that draw.

---

## Game A: Mariage (Borlette Mariage)

### Definition

Player selects **two 2-digit numbers**. The draw publishes **three 2-digit winning borlette numbers** (e.g. `["12","34","56"]`).

### Win condition (recommended baseline)

- Win if **both chosen numbers appear** anywhere in the three winning borlette numbers.
    
- Order does not matter.
    

### Input

- Two numbers: `a`, `b`
    
- Each must be 2 digits (`"00"`–`"99"`)
    

### Validation

- `a` and `b` must be valid 2-digit strings after normalization.
    
- Decide if duplicates are allowed:
    
    - **Recommended: disallow duplicates** (`a !== b`) to avoid “double counting” confusion.
        
    - If you allow duplicates, define how it wins:
        
        - would require the winning list to contain that number twice (rare). Not recommended.
            

### Normalization

- `a = pad2(clean(a))`
    
- `b = pad2(clean(b))`
    

### pickKey

Order-invariant:

- sort lexicographically: `min-max`
    
- `pickKey = "${x}-${y}"`
    

Example:

- input: `("4", "12")` → normalized: `("04", "12")` → pickKey: `"04-12"`
    

### Evaluation

- winning set: `W = Set(borlette3)`
    
- win if `(a in W) && (b in W)`
    
- win amount: `stake * multiplier`
    

### Suggested default multiplier (from your draft)

- `multiplier = 1000`
    

### Tests

- ✅ win: pick `12-34`, result `["12","34","56"]`
    
- ❌ lose: pick `12-99`, result `["12","34","56"]`
    
- ✅ leading zeros: pick `4-12` should match `"04-12"`
    
- ❌ invalid: pick `a="1a"` or `b="123"` rejected
    
- ❌ duplicates: `12-12` rejected (recommended)
    

---

## Game B: Lotto 3

### Definition

Player selects a **3-digit number**.

### Options (keep it simple for v1)

- **EXACT**: match all 3 digits in exact order.
    

(You can later add BOX/ANY_ORDER as a separate option if needed.)

### Input

- `n`: 3 digits
    

### Validation

- digits-only, length 1–3 before padding
    
- normalized to 3 digits
    

### pickKey

- `pickKey = lotto3` normalized exact string (e.g. `"007"`)
    

### Result requirement

- `drawResult.numbers.lotto3` must exist and be `"000"`–`"999"`
    

### Evaluation

- win if `pickKey === result.lotto3`
    
- win amount = `stake * multiplier`
    

### Suggested default multiplier (your draft)

- `multiplier = 1000`
    

### Tests

- ✅ win: pick `"123"`, result `"123"`
    
- ✅ win: pick `"7"` normalized to `"007"`, result `"007"`
    
- ❌ lose: pick `"123"`, result `"124"`
    
- ❌ invalid: `"12a"` rejected
    

---

## Game C: Lotto 4 (with Option 1/2/3)

Because you’re unsure about the three options, here’s a robust set that covers common “option” patterns while remaining simple and deterministic.

### Lotto 4 options (proposed)

1. **STRAIGHT (OPT1)**: exact 4 digits in order (highest payout)
    
2. **BOX (OPT2)**: any permutation of the 4 digits matches (lower payout)
    
3. **FRONTBACK (OPT3)**: match either first 2 digits OR last 2 digits (lowest payout)
    

These are easy for users, easy for vendors, and easy for settlement logic. If your Haiti-specific options differ, you can swap the match rule but keep the same schema design.

### Input

- `n`: 4 digits
    

### Validation

- digits-only
    
- normalized to 4 digits
    

### pickKey by option

- OPT1 STRAIGHT: `pickKey = "S:" + n4`
    
- OPT2 BOX: `pickKey = "B:" + sortDigits(n4)`
    
    - e.g. `"1203"` → digits `[1,2,0,3]` → sorted `"0123"` → `B:0123`
        
- OPT3 FRONTBACK: `pickKey = "F:" + first2(n4) + "|L:" + last2(n4)`
    
    - e.g. `"1203"` → `F:12|L:03`
        

### Result requirement

- `drawResult.numbers.lotto4` exists and is 4-digit string
    

### Evaluation

Let `R = result.lotto4`.

- OPT1: win if `n4 === R`
    
- OPT2: win if `sortDigits(n4) === sortDigits(R)`
    
- OPT3: win if `first2(n4) === first2(R)` OR `last2(n4) === last2(R)`
    

### Multipliers

You can keep your draft multiplier for OPT1 and define others:

- OPT1 (straight): 50,000× (your draft)
    
- OPT2 (box): pick something lower (because odds are better)
    
- OPT3 (front/back): lower again
    

**Important:** Your multipliers are extremely high relative to typical lotteries. That’s okay if that’s the market, but you’ll need **exposure limits** to avoid insolvency.

### Tests

- OPT1 ✅: pick 0123, result 0123
    
- OPT2 ✅: pick 0123, result 3210
    
- OPT2 ❌: pick 0123, result 3211
    
- OPT3 ✅: pick 1203, result 1299 (front matches)
    
- OPT3 ✅: pick 1203, result 9999? (no)
    
- OPT3 ✅: pick 1203, result 9903 (back matches)
    

---

## Game D: Lotto 5 (with Option 1/2/3)

Mirror Lotto4 patterns for consistency.

### Lotto 5 options (proposed)

1. **STRAIGHT (OPT1)**: exact 5 digits
    
2. **BOX (OPT2)**: any permutation (note: many duplicates possible with repeated digits)
    
3. **PREFIXSUFFIX (OPT3)**: match first 3 digits OR last 3 digits
    

### Input

- `n`: 5 digits
    

### pickKey by option

- OPT1 STRAIGHT: `S:${n5}`
    
- OPT2 BOX: `B:${sortDigits(n5)}`
    
- OPT3 PREFIXSUFFIX: `P:${first3(n5)}|S:${last3(n5)}`
    

### Result requirement

- `drawResult.numbers.lotto5` exists and is 5-digit string
    

### Evaluation

- OPT1: win if `n5 === R`
    
- OPT2: win if `sortDigits(n5) === sortDigits(R)`
    
- OPT3: win if `first3(n5) === first3(R)` OR `last3(n5) === last3(R)`
    

### Multipliers

- OPT1: 100,000× (your draft)
    
- OPT2/OPT3 lower (configurable)
    

### Tests

- OPT1 ✅: 00123 matches 00123
    
- OPT2 ✅: 00123 matches 32010
    
- OPT3 ✅: 12345 matches 12399 (prefix matches)
    
- OPT3 ✅: 12345 matches 00945 (suffix matches)
    

---

## Canonical helper specs (use in both UI + server)

### Normalizers

- `normalizeDigits(raw, len)`:
    
    - strip spaces, strip separators, validate digits-only
        
    - if length > len → invalid
        
    - left pad with zeros to len
        

### sortDigits

- converts `"1203"` → `"0123"`
    

### pickKey builders (single source of truth)

- mariage: `M:${min2}-${max2}`
    
- lotto3 exact: `L3:S:${n3}`
    
- lotto4:
    
    - `L4:S:${n4}`
        
    - `L4:B:${sorted}`
        
    - `L4:FB:${first2}|${last2}`
        
- lotto5:
    
    - `L5:S:${n5}`
        
    - `L5:B:${sorted}`
        
    - `L5:PS:${first3}|${last3}`
        

Prefixing like this prevents collisions across games/options.

---

## Winner evaluation algorithm (settlement)

### Inputs

- `drawId`
    
- `drawResult.numbers` JSON
    
- ticket lines for tickets in that draw with status CONFIRMED
    

### Output

- update each `TicketLine.winAmount`, `evaluatedAt`
    
- update `Ticket.totalPotentialPayout` (optional) and computed totals
    
- create `Settlement` summary
    

### Pseudocode

`for each confirmed ticket in draw:   totalWin = 0n   for each line in ticket.lines:     result = getResultForGame(line.game.kind, drawResult.numbers)     if result is missing:       mark line as not evaluated (or win=0 with reason)       continue      didWin = evaluate(line.game.kind, line.option.code, line.pickKey, result)     if didWin:       win = line.stakeAmount * BigInt(line.payoutMultiplier)     else:       win = 0n      update line.winAmount = win, evaluatedAt = now     totalWin += win    update ticket aggregate if you keep it (optional)`

### Determinism rule

Evaluation must depend only on:

- `line.pickKey`
    
- `line.optionId` (or option code)
    
- `game.kind`
    
- `drawResult.numbers`
    
- `line.payoutMultiplier`
    
- `line.stakeAmount`
    

No other hidden state.

---

## UX mapping (how the rules show up in the Ticket Builder)

### Consumer “Buy” (simple)

- Step 1: choose draw
    
- Step 2: choose game (Mariage / Lotto3 / Lotto4 / Lotto5)
    
- Step 3: enter number(s)
    
    - Mariage: two 2-digit inputs
        
    - Lotto3/4/5: one input
        
- Step 4: choose option (only for Lotto4/5; show as:
    
    - “Exact”
        
    - “Any order”
        
    - “Front/Back” or “Prefix/Suffix”  
        )
        
- Step 5: stake amount
    
- Step 6: summary + confirm
    

### Vendor “Sell” (fast)

- One screen with:
    
    - draw selector (big)
        
    - game tabs
        
    - numeric keypad input
        
    - lines list (editable)
        
    - total stake
        
    - confirm + print
        

---

## One more thing you should lock now: exposure keys

Your schema has an `Exposure` model keyed by `(operatorOrgId, vendorOrgId, drawId, gameId, pickKey)`.

That means:

- **pickKey must be canonical and stable**, or your risk controls won’t work.
    
- The prefixes I proposed (`L4:S:`, etc.) make this safe.
    

---

If you tell me what **Option 1/2/3** truly mean in Haitian Lotto 4/5 (even a rough description), I’ll rewrite the option rules and pickKey patterns to match the real-world rules while keeping the same clean schema and settlement structure.