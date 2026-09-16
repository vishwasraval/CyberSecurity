# Cybersecurity Awareness Demo – QR Form Data Collection

This is a **demonstration** for a cybersecurity / awareness talk.  
It shows how a simple-looking registration form can collect more than the user expects (location + browser/device fingerprint) and can attempt to send that data to the email the user just entered.

## What this demo does

1. User scans QR → opens a clean registration form (Name, Email, Designation).
2. On Submit:
   - Browser asks for **location permission**.
   - Collects available device/browser info (CPU cores, approximate RAM, GPU, screen, timezone, user-agent, etc.).
   - Shows a clear report of everything that was collected.
3. Optional button opens the user’s default mail client (`mailto:`) already filled with the collected data and addressed to the email the user typed. This demonstrates “sending” without any backend.

### Important limitations (use these in your talk)

| Information                | Can browser get it? | Notes |
|---------------------------|---------------------|-------|
| Exact CPU model           | ❌ No               | Privacy protection |
| Exact RAM size            | ≈ Approximate only  | `navigator.deviceMemory` (Chrome, rounded) |
| Full disk / storage capacity | ❌ No            | Only browser origin quota |
| Location                  | ✅ Yes (with permission) | Geolocation API |
| CPU cores                 | ✅ Yes              | `hardwareConcurrency` |
| GPU vendor/renderer       | ✅ Yes (via WebGL)  | |
| Screen, OS, browser, timezone | ✅ Yes         | Standard fingerprinting |

This is **exactly** the point of the demo: browsers already block the most sensitive hardware details, but a surprising amount is still available, and location requires only one click of “Allow”.

## How to deploy on GitHub Pages (free)

1. Create a new GitHub repository (public).
2. Upload the single file `index.html` (or put it in a folder).
3. Go to **Settings → Pages**.
4. Under “Source” choose **Deploy from a branch** → `main` / `root` (or `/docs` if you prefer).
5. Wait 1–2 minutes. Your site will be live at:
   ```
   https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
   ```
6. Copy that exact URL.

## Generate the real QR code

Replace the placeholder URL with your live GitHub Pages URL and regenerate:

```bash
# On any machine with Python + qrcode
pip install qrcode[pil]
python3 -c "
import qrcode
url = 'https://YOUR-USERNAME.github.io/YOUR-REPO/'   # ← paste your real URL
qr = qrcode.QRCode(error_correction=qrcode.constants.ERROR_CORRECT_H, box_size=12, border=4)
qr.add_data(url)
qr.make(fit=True)
img = qr.make_image(fill_color='black', back_color='white')
img.save('qrcode.png')
print('QR saved as qrcode.png')
"
```

Or use any free online QR generator (e.g. qr-code-generator.com) and paste the GitHub Pages URL.

Print the QR or display it on a slide.

## Running the demo on Windows 11

1. Open the QR on the laptop (or have the audience scan it with their phones).
2. Fill the form with a test name + a real email you control.
3. Click Submit → you will see the **location permission** popup. Click Allow (or Deny to show both cases).
4. The page immediately shows the full collected report.
5. Click the green “Open Email Client” button → Outlook / Windows Mail / default client opens with the data already filled and addressed to the email you entered.

You can also open the page directly in Edge/Chrome without the QR for a controlled walkthrough.

## Optional: Real email sending (EmailJS)

If you want the form to actually send an email without opening the mail client:

1. Create a free account at https://www.emailjs.com
2. Connect a Gmail / Outlook service.
3. Create a template that uses `{{to_email}}`, `{{name}}`, `{{message}}` etc.
4. Add the EmailJS SDK script and your public key / service ID / template ID into `index.html`.

The current version uses the pure `mailto:` approach so the demo works offline and needs zero configuration.

## Talk talking points

- “This form looks completely normal.”
- “As soon as you click Submit, the browser is asked for your precise location.”
- “Even if you deny location, the site still learned your approximate RAM, number of CPU cores, GPU, screen size, timezone, and browser fingerprint.”
- “In a real phishing page the data would be sent to an attacker’s server instead of opening your mail client.”
- “This is why we teach people to be careful with permissions and with forms that ask for email + other details.”

---

**Disclaimer**: This project is for educational / cybersecurity awareness purposes only. Do not use it for any malicious activity.
