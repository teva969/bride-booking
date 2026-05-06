# Eternal Moments - Bridal Photography Booking

A single-page static website for a bridal photography studio, featuring a booking form, gallery, and package information.

## Live Preview

Serve locally with:

```bash
python3 -m http.server 8080
```

Then open [http://localhost:8080](http://localhost:8080).

## Features

- Responsive design with a blush-pink and gold palette
- About, Packages, Gallery, and Booking sections
- Contact form powered by [formsubmit.co](https://formsubmit.co) (no backend required)
- Smooth scroll navigation

## Structure

The entire site is a single file — `index.html` — with all HTML, CSS, and JavaScript inline. No build system or dependencies.

| Section | Description |
|---------|-------------|
| Hero | Full-screen landing with CTA |
| About | Studio introduction |
| Packages | Pricing tiers |
| Gallery | Photo grid |
| Booking | Contact/booking form |
| Contact | Address and contact details |

## Customization

- **Email**: Change `qingru@gmail.com` in the `<script>` block to update the form recipient.
- **Photos**: Swap Unsplash URLs in the HTML (`?w=&h=&fit=crop` params control sizing).
- **Colors**: Edit CSS custom properties at the top of the `<style>` block (`--blush`, `--rose`, `--dark-rose`, `--gold`, `--cream`).

## Form Submission

Uses [formsubmit.co](https://formsubmit.co) AJAX endpoint. On the very first submission, formsubmit.co sends an activation email to the recipient — that link must be clicked before real submissions are delivered.
