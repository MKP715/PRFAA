# PRF 2028 Website — Setup & Maintenance Guide

The whole site lives in a single file, `index.html`. It is a client-side single-page app, so it runs on GitHub Pages with no server.
Almost everything the host committee will need to change is in the **`PRF_CONFIG` block** near the bottom of `index.html` (search for `SITE CONFIGURATION`).

---

## 1. Publish on GitHub Pages

1. Commit `index.html`, `banner.jpeg` and the PDFs you want public to the `main` branch.
2. On GitHub, go to **Settings → Pages → Build and deployment**. Set Source to "Deploy from a branch", Branch to `main`, and folder to `/ (root)`.
3. The site appears at `https://<user-or-org>.github.io/<repo>/` within a minute or two.
4. *(Optional)* Add a custom domain such as `prf2028.msca09aa.org` under **Settings → Pages → Custom domain**, then create a CNAME record with your DNS provider.
5. Once you know the final URL, change the `og:image` meta tag at the top of `index.html` to an absolute URL (for example `https://…/banner.jpeg`) so link previews show the image.

Pages use hash links, e.g. `…/#/volunteer` or `…/#/flyer`. `?lang=es` opens the site in Spanish, e.g. `…/?lang=es#/flyer`.

---

## 2. Build the Google Forms (4 forms)

The site currently embeds a **public sample form as a placeholder**, and every form shows a yellow "Placeholder form" notice. For each form below:

1. Create the form at <https://forms.google.com>, signed in with a **committee/service Google account**, not a personal one.
2. **Settings:**
   - Responses → *Collect email addresses*: "Responder input". This avoids requiring a Google sign-in.
   - Responses → *Limit to 1 response*: **OFF**, because it forces a Google sign-in.
   - Presentation → confirmation message (see each form below).
3. **Responses tab:** link to a Google Sheet, then click ⋮ → *Get email notifications for new responses*.
4. Click **Send → 🔗 link**, copy the long URL that ends in `/viewform`, and **don't** use the shortened `forms.gle` link.
5. In `index.html` → `PRF_CONFIG.forms.<name>`, paste the URL into `url` and set `demo: false`.
6. Optional: adjust `height` (in pixels) so the embedded form doesn't show an inner scrollbar.

> Tip: make each form bilingual by writing each question as `English / Español`, e.g. "First name / Nombre".
> Anonymity: ask for **first name and last initial** only. Never require a full name.

### 2a. Volunteer & Service Sign-up (`forms.volunteer`)

| # | Question | Type | Required | Options / notes |
|---|----------|------|----------|-----------------|
| 1 | First name & last initial / Nombre e inicial del apellido | Short answer | ✔ | |
| 2 | Email / Correo electrónico | (collected by setting) | ✔ | |
| 3 | Phone (optional) / Teléfono (opcional) | Short answer | | Response validation: text length ≤ 20 |
| 4 | Area / Área | Dropdown | ✔ | 02 Alaska, 03 Arizona, 05 Southern California, 06 CNCA, 07 CNIA, 08 San Diego-Imperial, **09 Mid-Southern California**, 17 Hawaii, 18 Idaho, 42 Nevada, 58 Oregon, 69 Utah, 72 Western Washington, 92 Eastern Washington, 93 Central California, Other / Otra |
| 5 | District / Distrito | Short answer | | |
| 6 | Current service position (if any) / Puesto de servicio actual | Short answer | | e.g. GSR, DCM, committee chair |
| 7 | **Position of interest / Puesto de interés** | Checkboxes | ✔ | Registration Chair · Carry the Message Chair · Volunteers/Greeters Chair · Translation Equipment Chair · Workshops Chair · A.A. Meetings Chair · Coffee Chair · Friday/Saturday Night Activities Chair · Hospitality Suite Chair · Attraction/Outreach Committee · **General weekend volunteer** · Wherever I'm needed |
| 8 | Availability during Forum weekend / Disponibilidad | Checkboxes | | Thu Nov 30 · Fri Dec 1 morning · Fri Dec 1 afternoon · Fri Dec 1 evening · Sat Dec 2 morning · Sat Dec 2 afternoon · Sat Dec 2 evening · Sun Dec 3 morning |
| 9 | Languages / Idiomas | Checkboxes | | English · Español · Français · ASL · Other |
| 10 | Can you attend planning meetings starting Jan 2027? | Multiple choice | | Yes · Some · No |
| 11 | Relevant experience / Experiencia | Paragraph | | |
| 12 | Anything else? / ¿Algo más? | Paragraph | | |

**Confirmation message:** "Thank you for raising your hand! A committee member will contact you. Chair positions are confirmed by Area 09 at the January 2027 Assembly. / ¡Gracias por ofrecerse! Un miembro del comité se comunicará con usted."

**Pre-filling the position (optional, recommended):** when someone clicks "I'm interested" on a position card, the site can pre-select that position in the form.
1. In the form editor, click ⋮ → **Get pre-filled link**, tick any position under question 7, then click **Get link** → copy.
2. The link contains something like `entry.1234567890=Registration+Chair`.
3. Set `PRF_CONFIG.forms.volunteer.prefillEntry` to `'entry.1234567890'`.
4. The option labels in question 7 must match the English position titles on the site **exactly**: Registration Chair, Carry the Message Chair, Volunteers / Greeters Chair, Translation Equipment Chair, Workshops Chair, A.A. Meetings Chair, Coffee Chair, Friday/Saturday Night Activities Chair, Hospitality Suite Chair, Attraction / Outreach Committee.

### 2b. Stay Informed — Updates (`forms.updates`)

| # | Question | Type | Required | Options |
|---|----------|------|----------|---------|
| 1 | First name & last initial | Short answer | ✔ | |
| 2 | Email | (collected by setting) | ✔ | |
| 3 | Area | Dropdown | ✔ | same list as above |
| 4 | Preferred language / Idioma preferido | Multiple choice | ✔ | English · Español |
| 5 | Do you plan to attend? / ¿Planea asistir? | Multiple choice | | Yes · Maybe · Online if available · Just want updates |
| 6 | Will this be your first Forum? | Multiple choice | | Yes · No |
| 7 | I would like updates about | Checkboxes | | Registration opening · Hotel booking · Volunteering · Ride/room share · Special events |

This form doubles as an early head-count, which helps the committee plan coffee, hospitality and outreach.

### 2c. Questions, Ideas & Presentation Requests (`forms.contact`)

| # | Question | Type | Required | Options |
|---|----------|------|----------|---------|
| 1 | First name & last initial | Short answer | ✔ | |
| 2 | Email | (collected by setting) | ✔ | |
| 3 | Topic / Tema | Multiple choice | ✔ | General question · Accessibility need · Idea / suggestion · **Presentation request** · Volunteering · Other |
| 4 | Area / District / Group | Short answer | | |
| 5 | For presentation requests: date, time, location & audience | Paragraph | | Use section logic, or leave it optional |
| 6 | Message / Mensaje | Paragraph | ✔ | |

### 2d. Ride Share & Room Share (`forms.rideshare`)

| # | Question | Type | Required | Options |
|---|----------|------|----------|---------|
| 1 | First name & last initial | Short answer | ✔ | |
| 2 | Email | (collected by setting) | ✔ | |
| 3 | I am… | Multiple choice | ✔ | Offering a ride · Looking for a ride · Offering to share a room · Looking for a roommate |
| 4 | Area / city travelling from | Short answer | ✔ | |
| 5 | Dates | Checkboxes | | Thu Nov 30 · Fri Dec 1 · Sat Dec 2 · Sun Dec 3 · Mon Dec 4 |
| 6 | Seats available / needed, or room preferences | Paragraph | | |
| 7 | I agree that the host committee may share my first name, area and email with a matched member | Checkbox | ✔ | |

The committee matches people manually from the response sheet. Do **not** publish the sheet: it contains personal contact details.

---

## 3. Keep the content current

Everything below is in `PRF_CONFIG` (plus two text files noted):

| When | What to change |
|------|----------------|
| After the Jan 2027 Assembly | `commitmentStatus`: set filled positions to `'filled'` |
| As meetings are scheduled | `committeeMeetings`: add `{ date: 'Jan 16, 2027 · 10 am', info: { en: '…', es: '…' } }` |
| GSO opens registration (~fall 2028) | `links.registration`, `hotel.rate`, `hotel.bookingUrl`, `hotel.groupCode`, `hotel.cutoff`; update the "Registration is not open yet" wording (`home.regTitle` / `home.regBody` in both languages) |
| Official program released | Update `schedule` in `PRF_DATA` (part B) |
| Contact changes | `contact` (set `showPhone: true` to show the phone number) |
| Proposed extras confirmed or dropped | `extras` and the `proposed: true` flags in `schedule` |

Most text is stored twice, `en` and `es`. Please update both.

---

## 4. Items to verify before publicizing

- **Hotel contract dates.** The booklet's contract summary lists room nights **Thu 09/28–Sat 09/30/2028**, while the Forum is **Dec 1–3, 2028**. Please confirm with GSO / the hotel.
- **Contract information in the repo.** `PACIFIC REGIONAL FORUM.pdf` includes contract pricing, estimated costs and the hotel sales manager's contact details. The website does **not** link to it, but anything committed to a public GitHub repo is downloadable. Consider removing it from the repo (and its git history) before making the repo public.
- **Slide deck footer.** `Pacific Regional Forum - 2028 .pdf` (linked from the site) still has template text ("Confidential · Customized for Lorem Ipsum LLC"). Consider re-exporting it without that text.
- **Contact email.** The chair's personal email is shown publicly. A dedicated address (e.g. `prf2028@…`) protects the chair's privacy and can be handed off after rotation.
- **Fun facts.** Past Area 09 hosting (1998 Santa Clara, 2006 Banning) comes straight from the booklet. Please double-check it.
- **Proposed extras.** Hybrid broadcast, app, Thursday event, banquet, LACYPAA after-party and area hospitality are all labeled "Proposed" on the site until confirmed.
- **Interpretation.** The site says interpretation is "typically" provided. Confirm the details with GSO.

---

## 5. Open-source libraries used (via CDN)

Bootstrap 5.3 · Bootstrap Icons · Alpine.js (routing/state) · Leaflet + OpenStreetMap (maps) · AOS (scroll animations) · Day.js (countdown) · Fuse.js (search) · add-to-calendar-button · qrcodejs (flyer QR) · html2pdf.js / html2canvas (flyer PDF & PNG, loaded on demand) · DOMPurify · Google Fonts (Raleway, Inter).
