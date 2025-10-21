import React, { useEffect, useState } from "react";
import { Check, Upload, Car, Shield, PhoneCall, Star, ArrowRight, X } from "lucide-react";

// --- External payment & booking links (replace with your live URLs) ---
const STRIPE_CHECKOUT = {
  single: "https://buy.stripe.com/live_1dealAbCdEf", // $49 plan
  bundle: "https://buy.stripe.com/live_5dealsXyZ",  // $99 plan
  pro: "https://buy.stripe.com/live_pro12345"       // fallback if not using Calendly
};

// Where users should land after successful Stripe payment (configure in Stripe Checkout Session settings)
// Example if deployed on: https://cardealrater.com
const SUCCESS_URL = "https://cardealrater.com/thank-you";
const CANCEL_URL = "https://cardealrater.com/";

// Calendly (Pro): create a paid session (with Stripe) and add a post-booking redirect to /booked
const CALENDLY_PRO = "https://calendly.com/cardealrater/pro-session";
const CALENDLY_SUCCESS_URL = "https://cardealrater.com/booked";

function classNames(...classes) {
  return classes.filter(Boolean).join(" ");
}

const plans = [
  { id: "single", name: "Deal Check (1)", price: "$49", blurb: "Rate one deal fast. Upload a photo or enter the numbers.",
    features: ["Detailed deal score (A–F)", "MSRP vs Selling vs OTD analysis", "Hidden fees scan", "Delivery/ADM check"], cta: "Start — $49" },
  { id: "bundle", name: "Value Pack (5) + Trade-In", price: "$99", blurb: "Five deal checks plus a fair trade-in estimate.",
    features: ["5 separate deal ratings", "Trade-in value range", "Market comps snapshot", "Email summary for each"], cta: "Start — $99", highlight: true },
  { id: "pro", name: "Live Specialist + 2 Carfax", price: "$499", blurb: "White-glove help: we join calls, text dealers, and de-risk add-ons.",
    features: ["1:1 live support (phone/text)", "2 Carfax reports included", "Finance & add-on audit", "Negotiation gameplan"], cta: "Book — $499" },
];

// --- Minimal router (no libs): render by pathname ---
export default function AppRouter() {
  const path = typeof window !== 'undefined' ? window.location.pathname : '/';
  if (path === '/thank-you') return <ThankYou />;
  if (path === '/booked') return <Booked />;
  return <CarDealRaterLanding />;
}

function CarDealRaterLanding() {
  const [open, setOpen] = useState(false);
  const [selectedPlan, setSelectedPlan] = useState(plans[1]);
  const [file, setFile] = useState(null);

  return (
    <div className="min-h-screen bg-white text-neutral-900 antialiased">
      <SiteHeader onPrimary={() => (window.location.href = STRIPE_CHECKOUT.bundle)} />

      {/* Hero */}
      <section className="mx-auto max-w-6xl px-4 py-20 md:py-28">
        <div className="grid md:grid-cols-2 gap-10 items-center">
          <div>
            <h1 className="text-4xl md:text-5xl font-semibold tracking-tight leading-tight">
              Buy smart. <span className="text-neutral-500">Never overpay.</span>
            </h1>
            <p className="mt-5 text-lg text-neutral-700">
              Upload the dealer quote or enter the numbers. Get an instant score,
              clean breakdown, and exact action steps. Minimal. Accurate. Brutal honesty.
            </p>
            <div className="mt-8 flex flex-wrap gap-3">
              <button onClick={() => (window.location.href = STRIPE_CHECKOUT.bundle)} className="rounded-2xl px-5 py-3 bg-black text-white hover:opacity-90">
                Start with Value Pack — $99
              </button>
              <a href="#pricing" className="rounded-2xl px-5 py-3 border border-neutral-300 hover:border-black">See pricing</a>
            </div>
            <div className="mt-6 flex items-center gap-4 text-sm text-neutral-600">
              <div className="flex items-center gap-2"><Shield className="h-4 w-4"/>Secure checkout</div>
              <div className="flex items-center gap-2"><Star className="h-4 w-4"/>Trusted by savvy buyers</div>
              <div className="flex items-center gap-2"><Car className="h-4 w-4"/>Any make & model</div>
            </div>
          </div>
          <div className="rounded-3xl border border-neutral-200 p-6 shadow-sm">
            <div className="aspect-video rounded-2xl bg-neutral-50 border border-neutral-200 flex items-center justify-center">
              <div className="text-center">
                <Upload className="h-8 w-8 mx-auto mb-3"/>
                <p className="font-medium">Drop your deal photo</p>
                <p className="text-sm text-neutral-600">or click a plan to type details</p>
              </div>
            </div>
            <ul className="mt-6 space-y-2 text-sm text-neutral-700">
              <li className="flex items-start gap-2"><Check className="h-4 w-4 mt-1"/>Score vs market pricing</li>
              <li className="flex items-start gap-2"><Check className="h-4 w-4 mt-1"/>Fees, add-ons, and OTD audit</li>
              <li className="flex items-start gap-2"><Check className="h-4 w-4 mt-1"/>Finance vs cash guidance</li>
            </ul>
          </div>
        </div>
      </section>

      {/* How it works */}
      <section id="how" className="mx-auto max-w-6xl px-4 py-6 md:py-10">
        <div className="grid md:grid-cols-3 gap-6">
          {["Upload or enter", "We rate it", "Act with clarity"].map((t, i) => (
            <div key={t} className="rounded-3xl border border-neutral-200 p-6">
              <div className="text-sm text-neutral-500">Step {i+1}</div>
              <div className="mt-1 text-lg font-medium">{t}</div>
              <p className="mt-2 text-neutral-700">
                {i===0 && "Add a photo of the quote or fill in year, make, model, MSRP, selling price, dealer fees, and extras."}
                {i===1 && "We compute a clean A–F score, market delta, and flag junk add-ons. Zero fluff."}
                {i===2 && "You get exact next steps to lower the price or lock it in. Fast, surgical, objective."}
              </p>
            </div>
          ))}
        </div>
      </section>

      {/* Pricing */}
      <section id="pricing" className="mx-auto max-w-6xl px-4 py-16 md:py-20">
        <h2 className="text-3xl md:text-4xl font-semibold tracking-tight text-center">Pricing</h2>
        <p className="text-center text-neutral-600 mt-3">Simple tiers. No upsells. No noise.</p>

        <div className="mt-10 grid md:grid-cols-3 gap-6">
          {plans.map((plan) => (
            <div key={plan.id} className={classNames("rounded-3xl border p-6 flex flex-col", plan.highlight ? "border-neutral-900 shadow-[0_8px_40px_-12px_rgba(0,0,0,0.25)]" : "border-neutral-200 shadow-sm")}
            >
              <div className="flex-1">
                <div className="text-sm text-neutral-500">{plan.name}</div>
                <div className="mt-1 text-3xl font-semibold">{plan.price}</div>
                <p className="mt-2 text-neutral-700">{plan.blurb}</p>
                <ul className="mt-4 space-y-2 text-sm text-neutral-700">
                  {plan.features.map((f) => (<li key={f} className="flex items-start gap-2"><Check className="h-4 w-4 mt-1"/>{f}</li>))}
                </ul>
              </div>
              <button
                onClick={() => plan.id === "pro" ? (window.location.href = CALENDLY_PRO) : (window.location.href = STRIPE_CHECKOUT[plan.id])}
                className={classNames("mt-6 rounded-2xl px-4 py-3 flex items-center justify-center gap-2", plan.highlight ? "bg-black text-white hover:opacity-90" : "border border-neutral-300 hover:border-black")}
              >
                {plan.cta} <ArrowRight className="h-4 w-4"/>
              </button>
            </div>
          ))}
        </div>
      </section>

      {/* Footer */}
      <SiteFooter />
    </div>
  );
}

// --- Thank You page (Stripe success_url) ---
function ThankYou() {
  return (
    <div className="min-h-screen bg-white text-neutral-900">
      <SiteHeader />
      <main className="mx-auto max-w-3xl px-4 py-24 text-center">
        <h1 className="text-4xl font-semibold tracking-tight">Payment received.</h1>
        <p className="mt-4 text-neutral-700">Check your email in a few minutes for instructions. If you uploaded a quote, we’ll begin the rating. If you entered details, you’ll get your score shortly.</p>
        <div className="mt-8">
          <a href="/" className="inline-block rounded-2xl px-5 py-3 bg-black text-white hover:opacity-90">Back to home</a>
        </div>
      </main>
      <SiteFooter />
    </div>
  );
}

// --- Booked page (Calendly redirect) ---
function Booked() {
  return (
    <div className="min-h-screen bg-white text-neutral-900">
      <SiteHeader />
      <main className="mx-auto max-w-3xl px-4 py-24 text-center">
        <h1 className="text-4xl font-semibold tracking-tight">Session booked.</h1>
        <p className="mt-4 text-neutral-700">You’ll receive a calendar invite and SMS confirmation. Bring your dealer quote(s) and any trade-in details to the call.</p>
        <div className="mt-8">
          <a href="/" className="inline-block rounded-2xl px-5 py-3 bg-black text-white hover:opacity-90">Back to home</a>
        </div>
      </main>
      <SiteFooter />
    </div>
  );
}

function SiteHeader({ onPrimary }) {
  return (
    <header className="sticky top-0 z-40 backdrop-blur supports-[backdrop-filter]:bg-white/70 bg-white/80 border-b border-neutral-200">
      <div className="mx-auto max-w-6xl px-4 py-4 flex items-center justify-between">
        <div className="flex items-center gap-2">
          <div className="h-7 w-7 rounded-2xl bg-black"></div>
          <a href="/" className="font-semibold tracking-tight">Car Deal Rater</a>
        </div>
        <nav className="hidden md:flex items-center gap-8 text-sm text-neutral-700">
          <a href="/" className="hover:text-black">Home</a>
          <a href="#how" className="hover:text-black">How it works</a>
          <a href="#pricing" className="hover:text-black">Pricing</a>
          <a href="#faq" className="hover:text-black">FAQ</a>
        </nav>
        {onPrimary ? (
          <button onClick={onPrimary} className="rounded-2xl px-4 py-2 bg-black text-white text-sm hover:opacity-90">Get started</button>
        ) : (
          <a href="/" className="rounded-2xl px-4 py-2 border border-neutral-300 text-sm hover:border-black">Home</a>
        )}
      </div>
    </header>
  );
}

function SiteFooter() {
  return (
    <footer className="border-t border-neutral-200">
      <div className="mx-auto max-w-6xl px-4 py-10 text-sm text-neutral-600 flex flex-col md:flex-row items-center justify-between gap-4">
        <div>© {new Date().getFullYear()} Car Deal Rater. All rights reserved.</div>
        <div className="flex items-center gap-6">
          <a className="hover:text-black" href="#">Privacy</a>
          <a className="hover:text-black" href="#">Terms</a>
          <a className="hover:text-black" href="#">Support</a>
        </div>
      </div>
    </footer>
  );
}

function Field({ label, placeholder }) {
  return (
    <label className="block">
      <span className="text-sm">{label}</span>
      <input type="text" placeholder={placeholder} className="mt-1 block w-full rounded-xl border border-neutral-300 px-3 py-2 focus:outline-none focus:ring-2 focus:ring-black/10" />
    </label>
  );
}
