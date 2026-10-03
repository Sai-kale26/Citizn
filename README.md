import React, { useMemo, useState } from "react";
import {
  AlertCircle,
  ArrowRight,
  Bell,
  Camera,
  CheckCircle2,
  ChevronDown,
  Clock3,
  CloudSun,
  FileText,
  Filter,
  Globe2,
  Heart,
  Home,
  ImagePlus,
  Info,
  Landmark,
  Layers3,
  ListFilter,
  LocateFixed,
  MapPin,
  Menu,
  MessageCircle,
  Moon,
  Navigation,
  Plus,
  Search,
  Send,
  Settings,
  ShieldCheck,
  Sparkles,
  Star,
  ThumbsUp,
  TrendingUp,
  Upload,
  User,
  X,
  Zap,
} from "lucide-react";

/* =========================================================
   SMART CIVIC — Civic Issue Reporting Platform
   React + TypeScript + Tailwind CSS
   Single-file frontend
   ========================================================= */

type Language = "EN" | "HI" | "MR";
type Status = "Reported" | "Assigned" | "In Progress" | "Resolved";

interface Issue {
  id: string;
  title: string;
  category: string;
  description: string;
  location: string;
  status: Status;
  priority: "Low" | "Medium" | "High";
  reported: string;
  votes: number;
  color: string;
  icon: string;
}

/* ----------------------- MOCK DATA ----------------------- */

const initialIssues: Issue[] = [
  {
    id: "CIV-1042",
    title: "Large pothole near Main Market",
    category: "Roads",
    description:
      "A large pothole has developed near the market entrance and is becoming difficult for two-wheelers.",
    location: "Main Market Road",
    status: "In Progress",
    priority: "High",
    reported: "2 hours ago",
    votes: 42,
    color: "#FF7A59",
    icon: "🛣️",
  },
  {
    id: "CIV-1041",
    title: "Streetlight not working",
    category: "Streetlights",
    description:
      "The streetlight outside the community park has been switched off for several days.",
    location: "Green Park",
    status: "Assigned",
    priority: "Medium",
    reported: "5 hours ago",
    votes: 27,
    color: "#F5B82E",
    icon: "💡",
  },
  {
    id: "CIV-1039",
    title: "Garbage collection missed",
    category: "Waste",
    description:
      "Waste has not been collected from the residential lane since yesterday morning.",
    location: "Shanti Nagar",
    status: "Reported",
    priority: "Medium",
    reported: "1 day ago",
    votes: 19,
    color: "#48B17B",
    icon: "🗑️",
  },
  {
    id: "CIV-1037",
    title: "Water leakage on footpath",
    category: "Water",
    description:
      "A continuous water leak is creating a slippery section on the pedestrian pathway.",
    location: "Station Road",
    status: "Resolved",
    priority: "High",
    reported: "2 days ago",
    votes: 64,
    color: "#4E9EF7",
    icon: "💧",
  },
  {
    id: "CIV-1033",
    title: "Broken park bench",
    category: "Public Spaces",
    description:
      "One of the benches near the children's play area is damaged.",
    location: "Central Garden",
    status: "Resolved",
    priority: "Low",
    reported: "4 days ago",
    votes: 11,
    color: "#A56CF4",
    icon: "🌳",
  },
];

const categories = [
  { name: "Roads", icon: "🛣️", color: "#FF7A59" },
  { name: "Streetlights", icon: "💡", color: "#F5B82E" },
  { name: "Waste", icon: "🗑️", color: "#48B17B" },
  { name: "Water", icon: "💧", color: "#4E9EF7" },
  { name: "Public Spaces", icon: "🌳", color: "#A56CF4" },
  { name: "Safety", icon: "🛡️", color: "#EC5A87" },
];

const translations = {
  EN: {
    dashboard: "Dashboard",
    explore: "Explore Issues",
    report: "Report Issue",
    myReports: "My Reports",
    community: "Community",
    welcome: "Good morning",
    subtitle: "Let's make your city a little better today.",
    reportButton: "Report an Issue",
    nearby: "Issues Near You",
    viewMap: "View Map",
    recent: "Recent Reports",
    all: "All",
    active: "Active",
    resolved: "Resolved",
    search: "Search civic issues...",
    category: "Category",
    status: "Status",
    submit: "Submit Report",
    location: "Location",
    description: "Description",
    issueTitle: "What needs attention?",
    upload: "Add photos",
    useLocation: "Use my location",
    impact: "Your civic impact",
    resolvedIssues: "Issues resolved",
    reports: "Reports submitted",
    helpful: "Community helpfulness",
    track: "Track your reports",
  },
  HI: {
    dashboard: "डैशबोर्ड",
    explore: "समस्याएँ देखें",
    report: "समस्या रिपोर्ट करें",
    myReports: "मेरी रिपोर्ट",
    community: "समुदाय",
    welcome: "सुप्रभात",
    subtitle: "आइए आज अपने शहर को थोड़ा बेहतर बनाएं।",
    reportButton: "समस्या रिपोर्ट करें",
    nearby: "आपके पास की समस्याएँ",
    viewMap: "मैप देखें",
    recent: "हाल की रिपोर्ट",
    all: "सभी",
    active: "सक्रिय",
    resolved: "हल की गई",
    search: "नागरिक समस्याएँ खोजें...",
    category: "श्रेणी",
    status: "स्थिति",
    submit: "रिपोर्ट भेजें",
    location: "स्थान",
    description: "विवरण",
    issueTitle: "किस चीज़ पर ध्यान चाहिए?",
    upload: "फोटो जोड़ें",
    useLocation: "मेरी लोकेशन इस्तेमाल करें",
    impact: "आपका नागरिक प्रभाव",
    resolvedIssues: "हल की गई समस्याएँ",
    reports: "जमा की गई रिपोर्ट",
    helpful: "समुदाय की मदद",
    track: "अपनी रिपोर्ट ट्रैक करें",
  },
  MR: {
    dashboard: "डॅशबोर्ड",
    explore: "समस्या शोधा",
    report: "समस्या नोंदवा",
    myReports: "माझ्या तक्रारी",
    community: "समुदाय",
    welcome: "शुभ सकाळ",
    subtitle: "चला, आज आपले शहर थोडे अधिक चांगले बनवूया.",
    reportButton: "समस्या नोंदवा",
    nearby: "तुमच्या जवळच्या समस्या",
    viewMap: "नकाशा पहा",
    recent: "अलीकडील तक्रारी",
    all: "सर्व",
    active: "सक्रिय",
    resolved: "निराकरण",
    search: "नागरी समस्या शोधा...",
    category: "श्रेणी",
    status: "स्थिती",
    submit: "तक्रार पाठवा",
    location: "स्थान",
    description: "वर्णन",
    issueTitle: "कशाकडे लक्ष देणे आवश्यक आहे?",
    upload: "फोटो जोडा",
    useLocation: "माझे स्थान वापरा",
    impact: "तुमचा नागरी प्रभाव",
    resolvedIssues: "निराकरण झालेल्या समस्या",
    reports: "नोंदवलेल्या तक्रारी",
    helpful: "समुदायाची मदत",
    track: "तुमच्या तक्रारी ट्रॅक करा",
  },
};

/* ----------------------- HELPERS ----------------------- */

const statusStyles: Record<Status, string> = {
  Reported: "bg-orange-50 text-orange-600 border-orange-100",
  Assigned: "bg-blue-50 text-blue-600 border-blue-100",
  "In Progress": "bg-violet-50 text-violet-600 border-violet-100",
  Resolved: "bg-emerald-50 text-emerald-600 border-emerald-100",
};

function StatusBadge({ status }: { status: Status }) {
  return (
    <span
      className={`inline-flex items-center gap-1.5 rounded-full border px-3 py-1 text-xs font-bold ${statusStyles[status]}`}
    >
      <span
        className={`h-1.5 w-1.5 rounded-full ${
          status === "Resolved"
            ? "bg-emerald-500"
            : status === "In Progress"
            ? "bg-violet-500"
            : status === "Assigned"
            ? "bg-blue-500"
            : "bg-orange-500"
        }`}
      />
      {status}
    </span>
  );
}

/* ----------------------- MAIN APP ----------------------- */

export default function App() {
  const [language, setLanguage] = useState<Language>("EN");
  const [activePage, setActivePage] = useState("dashboard");
  const [issues, setIssues] = useState<Issue[]>(initialIssues);
  const [showReport, setShowReport] = useState(false);
  const [showNotifications, setShowNotifications] = useState(false);
  const [showProfile, setShowProfile] = useState(false);
  const [mobileMenu, setMobileMenu] = useState(false);
  const [darkMode, setDarkMode] = useState(false);

  const t = translations[language];

  const navigate = (page: string) => {
    setActivePage(page);
    setMobileMenu(false);
  };

  return (
    <div
      className={
        darkMode
          ? "min-h-screen bg-[#101314] text-white"
          : "min-h-screen bg-[#F6F8F7] text-[#17201C]"
      }
    >
      <style>{`
        * { box-sizing: border-box; }
        body { margin: 0; font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; }
        ::selection { background: #b9f2d4; color: #153525; }
        .soft-shadow { box-shadow: 0 18px 60px rgba(30, 55, 43, .08); }
        .soft-shadow-sm { box-shadow: 0 8px 30px rgba(30, 55, 43, .06); }
        .glass { backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px); }
        .mesh {
          background:
            radial-gradient(circle at 10% 20%, rgba(82, 202, 137, .18), transparent 26%),
            radial-gradient(circle at 90% 10%, rgba(94, 177, 255, .16), transparent 26%),
            radial-gradient(circle at 70% 100%, rgba(255, 184, 77, .13), transparent 28%),
            #eef8f2;
        }
        .dark .mesh { background: #16211c; }
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { scrollbar-width: none; }
        .float { animation: float 5s ease-in-out infinite; }
        @keyframes float {
          0%,100% { transform: translateY(0); }
          50% { transform: translateY(-8px); }
        }
        .pulse-ring { animation: ring 2.2s infinite; }
        @keyframes ring {
          0% { box-shadow: 0 0 0 0 rgba(59, 185, 113, .35); }
          70% { box-shadow: 0 0 0 16px rgba(59, 185, 113, 0); }
          100% { box-shadow: 0 0 0 0 rgba(59, 185, 113, 0); }
        }
      `}</style>

      <div className="flex min-h-screen">
        {/* SIDEBAR */}
        <aside
          className={`fixed inset-y-0 left-0 z-40 w-[270px] border-r ${
            darkMode
              ? "border-white/10 bg-[#121816]"
              : "border-[#E2EAE5] bg-white"
          } transform transition-transform duration-300 lg:static lg:translate-x-0 ${
            mobileMenu ? "translate-x-0" : "-translate-x-full"
          }`}
        >
          <div className="flex h-full flex-col px-5 py-6">
            {/* Logo */}
            <button
              onClick={() => navigate("dashboard")}
              className="mb-8 flex items-center gap-3 px-2 text-left"
            >
              <div className="relative flex h-11 w-11 items-center justify-center rounded-2xl bg-[#183D2B] text-white shadow-lg shadow-emerald-900/20">
                <MapPin size={22} />
                <span className="absolute -right-1 -top-1 h-3 w-3 rounded-full border-2 border-white bg-[#62D395]" />
              </div>
              <div>
                <div className="text-lg font-black tracking-tight">
                  Civic<span className="text-[#38A968]">ly</span>
                </div>
                <div className="text-[10px] font-bold uppercase tracking-[.18em] text-slate-400">
                  Smart City
                </div>
              </div>
            </button>

            <div className="mb-3 px-3 text-[10px] font-black uppercase tracking-[.18em] text-slate-400">
              Main Menu
            </div>

            <nav className="space-y-1">
              {[
                { id: "dashboard", label: t.dashboard, icon: Home },
                { id: "explore", label: t.explore, icon: Layers3 },
                { id: "myreports", label: t.myReports, icon: FileText },
                { id: "community", label: t.community, icon: MessageCircle },
              ].map((item) => {
                const Icon = item.icon;
                const active = activePage === item.id;

                return (
                  <button
                    key={item.id}
                    onClick={() => navigate(item.id)}
                    className={`group flex w-full items-center gap-3 rounded-2xl px-4 py-3.5 text-sm font-bold transition ${
                      active
                        ? "bg-[#E7F7ED] text-[#218B51]"
                        : "text-slate-500 hover:bg-slate-50 hover:text-slate-900"
                    }`}
                  >
                    <Icon
                      size={19}
                      className={
                        active ? "text-[#2EAA65]" : "text-slate-400"
                      }
                    />
                    {item.label}
                    {item.id === "myreports" && (
                      <span className="ml-auto rounded-full bg-[#FFEFDF] px-2 py-0.5 text-[10px] text-orange-600">
                        3
                      </span>
                    )}
                  </button>
                );
              })}
            </nav>

            <div className="mt-8 mb-3 px-3 text-[10px] font-black uppercase tracking-[.18em] text-slate-400">
              Tools
            </div>

            <nav className="space-y-1">
              <button className="flex w-full items-center gap-3 rounded-2xl px-4 py-3.5 text-sm font-bold text-slate-500 hover:bg-slate-50">
                <TrendingUp size={19} className="text-slate-400" />
                City Insights
              </button>
              <button className="flex w-full items-center gap-3 rounded-2xl px-4 py-3.5 text-sm font-bold text-slate-500 hover:bg-slate-50">
                <ShieldCheck size={19} className="text-slate-400" />
                Safety Center
              </button>
              <button className="flex w-full items-center gap-3 rounded-2xl px-4 py-3.5 text-sm font-bold text-slate-500 hover:bg-slate-50">
                <Settings size={19} className="text-slate-400" />
                Settings
              </button>
            </nav>

            <div className="mt-auto">
              {/* Community card */}
              <div className="relative overflow-hidden rounded-[26px] bg-[#183D2B] p-5 text-white">
                <div className="absolute -right-8 -top-8 h-28 w-28 rounded-full bg-[#4FD084]/20" />
                <div className="absolute -bottom-10 -left-10 h-24 w-24 rounded-full bg-[#73A9FF]/10" />

                <Sparkles size={22} className="mb-4 text-[#75E0A4]" />
                <div className="mb-1 text-sm font-black">
                  Make an impact
                </div>
                <p className="mb-4 text-xs leading-5 text-white/65">
                  Every report helps your city become cleaner, safer and
                  smarter.
                </p>

                <button
                  onClick={() => setShowReport(true)}
                  className="flex w-full items-center justify-center gap-2 rounded-xl bg-[#64D695] px-3 py-2.5 text-xs font-black text-[#123321] transition hover:bg-[#7AE2A6]"
                >
                  <Plus size={15} />
                  {t.reportButton}
                </button>
              </div>

              <div className="mt-5 flex items-center gap-3 rounded-2xl bg-slate-50 p-3">
                <div className="flex h-10 w-10 items-center justify-center rounded-full bg-[#D8EFE1] font-black text-[#238851]">
                  AK
                </div>
                <div className="min-w-0 flex-1">
                  <div className="truncate text-xs font-black">Aarav Kumar</div>
                  <div className="truncate text-[10px] text-slate-400">
                    Citizen · Level 6
                  </div>
                </div>
                <ChevronDown size={14} className="text-slate-400" />
              </div>
            </div>
          </div>
        </aside>

        {/* MOBILE OVERLAY */}
        {mobileMenu && (
          <div
            onClick={() => setMobileMenu(false)}
            className="fixed inset-0 z-30 bg-black/30 backdrop-blur-sm lg:hidden"
          />
        )}

        {/* MAIN */}
        <main className="min-w-0 flex-1">
          {/* TOP BAR */}
          <header
            className={`sticky top-0 z-20 flex h-[76px] items-center justify-between border-b px-4 sm:px-6 lg:px-8 ${
              darkMode
                ? "border-white/10 bg-[#101314]/90"
                : "border-[#E6ECE8] bg-[#F6F8F7]/90"
            } glass`}
          >
            <div className="flex items-center gap-3">
              <button
                onClick={() => setMobileMenu(true)}
                className="rounded-xl p-2 hover:bg-slate-100 lg:hidden"
              >
                <Menu size={21} />
              </button>

              <div className="hidden items-center gap-2 text-xs font-bold text-slate-400 sm:flex">
                <MapPin size={14} className="text-[#32A966]" />
                <span>Ward 12</span>
                <span>•</span>
                <span>Nagpur</span>
                <ChevronDown size={13} />
              </div>
            </div>

            <div className="flex items-center gap-2 sm:gap-3">
              {/* Language */}
              <div className="relative">
                <select
                  value={language}
                  onChange={(e) =>
                    setLanguage(e.target.value as Language)
                  }
                  className="appearance-none rounded-xl border border-slate-200 bg-white py-2 pl-9 pr-7 text-xs font-black outline-none"
                >
                  <option value="EN">EN</option>
                  <option value="HI">हिंदी</option>
                  <option value="MR">मराठी</option>
                </select>
                <Globe2
                  size={14}
                  className="pointer-events-none absolute left-3 top-2.5 text-slate-400"
                />
              </div>

              <button
                onClick={() => setDarkMode(!darkMode)}
                className="hidden rounded-xl border border-slate-200 bg-white p-2.5 text-slate-500 transition hover:bg-slate-50 sm:block"
              >
                <Moon size={16} />
              </button>

              <div className="relative">
                <button
                  onClick={() =>
                    setShowNotifications(!showNotifications)
                  }
                  className="relative rounded-xl border border-slate-200 bg-white p-2.5 text-slate-500 hover:bg-slate-50"
                >
                  <Bell size={17} />
                  <span className="absolute right-2 top-2 h-2 w-2 rounded-full border border-white bg-[#F76F4F]" />
                </button>

                {showNotifications && (
                  <div className="absolute right-0 top-12 z-50 w-[320px] rounded-3xl border border-slate-100 bg-white p-3 shadow-2xl">
                    <div className="flex items-center justify-between px-3 py-2">
                      <span className="font-black">Notifications</span>
                      <span className="text-xs text-emerald-600">
                        2 new
                      </span>
                    </div>

                    {[
                      "Your pothole report was assigned.",
                      "A nearby streetlight issue was resolved.",
                      "You earned 20 community points.",
                    ].map((n, i) => (
                      <div
                        key={i}
                        className="mt-1 flex gap-3 rounded-2xl p-3 hover:bg-slate-50"
                      >
                        <div className="mt-1 h-2 w-2 shrink-0 rounded-full bg-emerald-500" />
                        <div>
                          <div className="text-xs font-bold text-slate-700">
                            {n}
                          </div>
                          <div className="mt-1 text-[10px] text-slate-400">
                            {i === 0 ? "12 min ago" : "1 hour ago"}
                          </div>
                        </div>
                      </div>
                    ))}
                  </div>
                )}
              </div>

              <button
                onClick={() => setShowProfile(!showProfile)}
                className="flex items-center gap-2 rounded-xl border border-slate-200 bg-white p-1.5 pr-3"
              >
                <div className="flex h-8 w-8 items-center justify-center rounded-lg bg-[#D8EFE1] text-xs font-black text-[#238851]">
                  AK
                </div>
                <span className="hidden text-xs font-black sm:block">
                  Aarav
                </span>
              </button>

              {showProfile && (
                <div className="absolute right-5 top-16 z-50 w-48 rounded-2xl border border-slate-100 bg-white p-2 shadow-xl">
                  <button className="w-full rounded-xl px-3 py-2 text-left text-xs font-bold hover:bg-slate-50">
                    My Profile
                  </button>
                  <button className="w-full rounded-xl px-3 py-2 text-left text-xs font-bold hover:bg-slate-50">
                    Account Settings
                  </button>
                  <button className="w-full rounded-xl px-3 py-2 text-left text-xs font-bold text-red-500 hover:bg-red-50">
                    Sign Out
                  </button>
                </div>
              )}
            </div>
          </header>

          {/* PAGE CONTENT */}
          <div className="mx-auto max-w-[1500px] p-4 sm:p-6 lg:p-8">
            {activePage === "dashboard" && (
              <Dashboard
                t={t}
                issues={issues}
                onReport={() => setShowReport(true)}
                onNavigate={navigate}
              />
            )}

            {activePage === "explore" && (
              <ExplorePage issues={issues} />
            )}

            {activePage === "myreports" && (
              <MyReports issues={issues.slice(0, 3)} />
            )}

            {activePage === "community" && (
              <CommunityPage issues={issues} />
            )}
          </div>
        </main>
      </div>

      {/* REPORT MODAL */}
      {showReport && (
        <ReportModal
          t={t}
          onClose={() => setShowReport(false)}
          onSubmit={(newIssue) => {
            setIssues((prev) => [newIssue, ...prev]);
            setShowReport(false);
          }}
        />
      )}
    </div>
  );
}

/* =========================================================
   DASHBOARD
   ========================================================= */

function Dashboard({
  t,
  issues,
  onReport,
  onNavigate,
}: {
  t: any;
  issues: Issue[];
  onReport: () => void;
  onNavigate: (page: string) => void;
}) {
  return (
    <>
      {/* HERO */}
      <section className="mesh soft-shadow relative overflow-hidden rounded-[34px] border border-white p-6 sm:p-8 lg:p-10">
        <div className="relative z-10 grid items-center gap-8 lg:grid-cols-[1.4fr_.6fr]">
          <div>
            <div className="mb-4 inline-flex items-center gap-2 rounded-full border border-[#CDEBDA] bg-white/70 px-3 py-1.5 text-[10px] font-black uppercase tracking-[.15em] text-[#318A56]">
              <span className="h-2 w-2 rounded-full bg-[#43C778] pulse-ring" />
              City pulse is active
            </div>

            <h1 className="max-w-[700px] text-3xl font-black leading-[1.08] tracking-[-.04em] text-[#163525] sm:text-4xl lg:text-5xl">
              {t.welcome}, Aarav.
              <br />
              <span className="text-[#38A968]">
                Let's improve our city.
              </span>
            </h1>

            <p className="mt-4 max-w-[600px] text-sm leading-6 text-[#527064] sm:text-base">
              {t.subtitle} Spot something that needs attention? Report it,
              follow its progress and help your neighbors stay informed.
            </p>

            <div className="mt-7 flex flex-wrap gap-3">
              <button
                onClick={onReport}
                className="group flex items-center gap-2 rounded-2xl bg-[#183D2B] px-5 py-3.5 text-sm font-black text-white shadow-xl shadow-emerald-900/10 transition hover:-translate-y-0.5 hover:bg-[#24583E]"
              >
                <Camera size={18} />
                {t.reportButton}
                <ArrowRight
                  size={16}
                  className="transition group-hover:translate-x-1"
                />
              </button>

              <button
                onClick={() => onNavigate("explore")}
                className="flex items-center gap-2 rounded-2xl border border-[#D5E7DC] bg-white/75 px-5 py-3.5 text-sm font-black text-[#315A45] transition hover:bg-white"
              >
                <Layers3 size={17} />
                {t.explore}
              </button>
            </div>
          </div>

          <div className="relative hidden min-h-[240px] lg:block">
            {/* Decorative city illustration */}
            <div className="absolute right-0 top-0 h-[230px] w-[330px]">
              <div className="absolute bottom-5 left-4 h-32 w-32 rounded-full bg-[#C7EAD4]" />
              <div className="absolute bottom-0 right-4 h-44 w-44 rounded-full bg-[#DDF2E5]" />

              <div className="absolute bottom-0 left-14 h-32 w-14 rounded-t-[14px] bg-[#8DC9A5]" />
              <div className="absolute bottom-0 left-28 h-44 w-20 rounded-t-[14px] bg-[#5BAE7B]" />
              <div className="absolute bottom-0 right-16 h-36 w-16 rounded-t-[14px] bg-[#76BE91]" />
              <div className="absolute bottom-0 right-2 h-24 w-14 rounded-t-[14px] bg-[#A5D8B8]" />

              <div className="absolute left-[78px] top-10 h-16 w-12 rounded-t-[24px] bg-[#F6B95D]" />
              <div className="absolute left-[92px] top-3 h-12 w-3 rounded-full bg-[#F6B95D]" />

              <div className="float absolute right-10 top-8 flex h-16 w-16 items-center justify-center rounded-[22px] bg-white shadow-xl">
                <MapPin className="text-[#31A967]" size={28} />
              </div>

              <div className="float absolute left-0 top-32 flex h-12 w-12 items-center justify-center rounded-2xl bg-white shadow-lg [animation-delay:1s]">
                <span className="text-xl">🌱</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      {/* STATS */}
      <section className="mt-6 grid grid-cols-2 gap-4 xl:grid-cols-4">
        <StatCard
          label={t.resolvedIssues}
          value="1,284"
          change="+12.4%"
          icon={<CheckCircle2 size={20} />}
          color="green"
        />
        <StatCard
          label={t.reports}
          value="342"
          change="+8.2%"
          icon={<FileText size={20} />}
          color="blue"
        />
        <StatCard
          label={t.helpful}
          value="94%"
          change="+4.7%"
          icon={<Heart size={20} />}
          color="pink"
        />
        <StatCard
          label="Avg. response"
          value="7.2h"
          change="-18%"
          icon={<Zap size={20} />}
          color="orange"
        />
      </section>

      {/* CONTENT GRID */}
      <section className="mt-6 grid gap-6 xl:grid-cols-[1.45fr_.55fr]">
        {/* ISSUES */}
        <div className="rounded-[30px] border border-[#E4EBE6] bg-white p-5 soft-shadow-sm sm:p-6">
          <div className="flex flex-wrap items-center justify-between gap-3">
            <div>
              <h2 className="text-xl font-black tracking-tight text-[#17201C]">
                {t.nearby}
              </h2>
              <p className="mt-1 text-xs text-slate-400">
                Based on your selected ward and recent activity
              </p>
            </div>

            <button
              onClick={() => onNavigate("explore")}
              className="flex items-center gap-1 text-xs font-black text-[#2D9D5D]"
            >
              {t.viewMap}
              <ArrowRight size={14} />
            </button>
          </div>

          <div className="mt-5 grid gap-3">
            {issues.slice(0, 4).map((issue) => (
              <IssueRow key={issue.id} issue={issue} />
            ))}
          </div>
        </div>

        {/* MAP */}
        <MiniMap />
      </section>

      {/* CATEGORIES */}
      <section className="mt-6">
        <div className="mb-4 flex items-end justify-between">
          <div>
            <h2 className="text-xl font-black tracking-tight">
              What needs fixing?
            </h2>
            <p className="mt-1 text-xs text-slate-400">
              Choose a category to report a civic issue quickly.
            </p>
          </div>
        </div>

        <div className="grid grid-cols-2 gap-3 sm:grid-cols-3 lg:grid-cols-6">
          {categories.map((cat) => (
            <button
              key={cat.name}
              onClick={onReport}
              className="group rounded-[24px] border border-[#E4EBE6] bg-white p-4 text-left transition hover:-translate-y-1 hover:border-[#BFE4CC] hover:shadow-xl hover:shadow-emerald-900/5"
            >
              <div
                className="mb-4 flex h-12 w-12 items-center justify-center rounded-2xl text-2xl transition group-hover:scale-110"
                style={{ background: `${cat.color}15` }}
              >
                {cat.icon}
              </div>
              <div className="text-sm font-black">{cat.name}</div>
              <div className="mt-1 text-[10px] text-slate-400">
                Report issue
              </div>
            </button>
          ))}
        </div>
      </section>

      {/* COMMUNITY */}
      <section className="mt-6 grid gap-6 lg:grid-cols-2">
        <div className="rounded-[30px] bg-[#183D2B] p-6 text-white soft-shadow">
          <div className="flex items-center justify-between">
            <div>
              <div className="text-xs font-bold uppercase tracking-[.15em] text-[#7BDDA1]">
                Community impact
              </div>
              <h3 className="mt-2 text-2xl font-black">
                Small reports.
                <br />
                Big difference.
              </h3>
            </div>
            <div className="flex h-14 w-14 items-center justify-center rounded-2xl bg-white/10">
              <Sparkles className="text-[#75E0A4]" />
            </div>
          </div>

          <div className="mt-7 grid grid-cols-3 gap-3">
            {[
              ["18.4K", "Reports"],
              ["12.7K", "Resolved"],
              ["6.2K", "Citizens"],
            ].map(([value, label]) => (
              <div
                key={label}
                className="rounded-2xl border border-white/10 bg-white/5 p-3"
              >
                <div className="text-lg font-black">{value}</div>
                <div className="mt-1 text-[10px] text-white/50">
                  {label}
                </div>
              </div>
            ))}
          </div>
        </div>

        <div className="rounded-[30px] border border-[#E4EBE6] bg-white p-6 soft-shadow-sm">
          <div className="flex items-center gap-3">
            <div className="flex h-11 w-11 items-center justify-center rounded-2xl bg-[#FFF0DB]">
              🏆
            </div>
            <div>
              <div className="text-xs font-bold text-[#F0A339]">
                Your civic score
              </div>
              <div className="text-2xl font-black">842 points</div>
            </div>
          </div>

          <div className="mt-6 h-3 overflow-hidden rounded-full bg-slate-100">
            <div className="h-full w-[72%] rounded-full bg-gradient-to-r from-[#4AC879] to-[#8FE1AD]" />
          </div>

          <div className="mt-2 flex justify-between text-[10px] font-bold text-slate-400">
            <span>Level 6</span>
            <span>1,000 pts to Level 7</span>
          </div>

          <div className="mt-5 flex items-center gap-3 rounded-2xl bg-[#F7FAF8] p-3">
            <div className="flex h-10 w-10 items-center justify-center rounded-xl bg-white">
              <Star size={18} className="fill-[#F5B82E] text-[#F5B82E]" />
            </div>
            <div className="flex-1">
              <div className="text-xs font-black">You're helping!</div>
              <div className="text-[10px] text-slate-400">
                4 reports received community support this week.
              </div>
            </div>
          </div>
        </div>
      </section>
    </>
  );
}

/* =========================================================
   STAT CARD
   ========================================================= */

function StatCard({
  label,
  value,
  change,
  icon,
  color,
}: {
  label: string;
  value: string;
  change: string;
  icon: React.ReactNode;
  color: string;
}) {
  const colors: Record<string, string> = {
    green: "bg-[#E8F8EE] text-[#2DA363]",
    blue: "bg-[#EAF3FF] text-[#4D98EA]",
    pink: "bg-[#FFF0F5] text-[#E46A94]",
    orange: "bg-[#FFF3E6] text-[#EAA13B]",
  };

  return (
    <div className="rounded-[26px] border border-[#E4EBE6] bg-white p-5 soft-shadow-sm">
      <div className="flex items-center justify-between">
        <div
          className={`flex h-10 w-10 items-center justify-center rounded-xl ${colors[color]}`}
        >
          {icon}
        </div>
        <span
          className={`rounded-full px-2 py-1 text-[10px] font-black ${
            change.startsWith("-")
              ? "bg-blue-50 text-blue-500"
              : "bg-emerald-50 text-emerald-600"
          }`}
        >
          {change}
        </span>
      </div>
      <div className="mt-5 text-2xl font-black tracking-tight">{value}</div>
      <div className="mt-1 text-xs font-bold text-slate-400">{label}</div>
    </div>
  );
}

/* =========================================================
   ISSUE ROW
   ========================================================= */

function IssueRow({ issue }: { issue: Issue }) {
  return (
    <div className="group flex gap-3 rounded-[22px] border border-transparent p-3 transition hover:border-[#E6EEE9] hover:bg-[#FBFDFC]">
      <div
        className="flex h-12 w-12 shrink-0 items-center justify-center rounded-2xl text-xl"
        style={{ background: `${issue.color}18` }}
      >
        {issue.icon}
      </div>

      <div className="min-w-0 flex-1">
        <div className="flex flex-wrap items-center gap-2">
          <h3 className="truncate text-sm font-black">{issue.title}</h3>
          <StatusBadge status={issue.status} />
        </div>

        <div className="mt-1 flex flex-wrap items-center gap-x-3 gap-y-1 text-[10px] font-bold text-slate-400">
          <span className="flex items-center gap-1">
            <MapPin size={11} />
            {issue.location}
          </span>
          <span>•</span>
          <span>{issue.reported}</span>
        </div>
      </div>

      <div className="hidden items-center gap-1 self-center rounded-xl bg-slate-50 px-2.5 py-2 text-xs font-black text-slate-500 sm:flex">
        <ThumbsUp size={13} />
        {issue.votes}
      </div>
    </div>
  );
}

/* =========================================================
   MINI MAP
   ========================================================= */

function MiniMap() {
  return (
    <div className="relative min-h-[390px] overflow-hidden rounded-[30px] border border-[#DCE9E0] bg-[#EAF3E9] soft-shadow-sm">
      <div
        className="absolute inset-0 opacity-60"
        style={{
          backgroundImage: `
            linear-gradient(35deg, transparent 45%, #ffffff 46%, #ffffff 49%, transparent 50%),
            linear-gradient(125deg, transparent 45%, #ffffff 46%, #ffffff 49%, transparent 50%),
            linear-gradient(90deg, transparent 49%, #d8e8dc 50%, transparent 51%),
            linear-gradient(0deg, transparent 49%, #d8e8dc 50%, transparent 51%)
          `,
          backgroundSize: "120px 120px, 170px 170px, 65px 65px, 80px 80px",
        }}
      />

      <div className="absolute left-[25%] top-[25%] h-20 w-32 rotate-12 rounded-[45%] bg-[#D6EAD8]" />
      <div className="absolute bottom-[18%] right-[18%] h-24 w-36 -rotate-12 rounded-[45%] bg-[#D1E7D5]" />

      {/* Map controls */}
      <div className="absolute left-4 top-4 flex flex-col overflow-hidden rounded-xl bg-white shadow-lg">
        <button className="p-2.5 text-slate-500 hover:bg-slate-50">
          <Plus size={16} />
        </button>
        <div className="h-px bg-slate-100" />
        <button className="p-2.5 text-slate-500 hover:bg-slate-50">
          <span className="text-lg leading-none">−</span>
        </button>
      </div>

      <button className="absolute right-4 top-4 rounded-xl bg-white p-2.5 text-slate-500 shadow-lg">
        <LocateFixed size={16} />
      </button>

      {/* Pins */}
      {[
        ["left-[30%] top-[32%]", "#FF7A59", "🛣️"],
        ["left-[55%] top-[23%]", "#F5B82E", "💡"],
        ["left-[68%] top-[57%]", "#4E9EF7", "💧"],
        ["left-[38%] top-[67%]", "#48B17B", "🗑️"],
        ["left-[78%] top-[32%]", "#A56CF4", "🌳"],
      ].map(([pos, color, emoji], i) => (
        <div
          key={i}
          className={`absolute ${pos} group cursor-pointer`}
        >
          <div
            className="flex h-10 w-10 items-center justify-center rounded-full border-4 border-white text-sm shadow-xl transition group-hover:scale-110"
            style={{ background: color }}
          >
            {emoji}
          </div>
          <div
            className="absolute left-1/2 top-full h-2 w-2 -translate-x-1/2 -translate-y-1 rounded-full"
            style={{ background: color }}
          />
        </div>
      ))}

      <div className="absolute bottom-4 left-4 right-4 flex items-center justify-between rounded-2xl border border-white/80 bg-white/85 p-3 shadow-lg glass">
        <div>
          <div className="text-xs font-black">24 issues nearby</div>
          <div className="mt-1 text-[10px] text-slate-400">
            Updated just now
          </div>
        </div>
        <button className="rounded-xl bg-[#183D2B] px-3 py-2 text-[10px] font-black text-white">
          Open map
        </button>
      </div>
    </div>
  );
}

/* =========================================================
   EXPLORE PAGE
   ========================================================= */

function ExplorePage({ issues }: { issues: Issue[] }) {
  const [query, setQuery] = useState("");
  const [filter, setFilter] = useState("All");

  const filtered = useMemo(() => {
    return issues.filter((issue) => {
      const matchesFilter =
        filter === "All" ||
        (filter === "Active" && issue.status !== "Resolved") ||
        (filter === "Resolved" && issue.status === "Resolved");

      const q = query.toLowerCase();

      return (
        matchesFilter &&
        (!q ||
          issue.title.toLowerCase().includes(q) ||
          issue.location.toLowerCase().includes(q) ||
          issue.category.toLowerCase().includes(q))
      );
    });
  }, [issues, query, filter]);

  return (
    <>
      <div className="mb-6">
        <div className="mb-2 text-xs font-black uppercase tracking-[.15em] text-[#36A667]">
          Explore
        </div>
        <h1 className="text-3xl font-black tracking-tight">
          What's happening around you?
        </h1>
        <p className="mt-2 max-w-xl text-sm text-slate-400">
          Browse reported civic issues, support problems that matter to your
          neighborhood and follow their progress.
        </p>
      </div>

      <div className="grid gap-6 xl:grid-cols-[.8fr_1.2fr]">
        <div className="relative min-h-[620px] overflow-hidden rounded-[32px] border border-[#DCE9E0] bg-[#EAF3E9]">
          <div
            className="absolute inset-0"
            style={{
              backgroundImage:
                "linear-gradient(45deg, transparent 48%, #fff 49%, #fff 51%, transparent 52%), linear-gradient(-35deg, transparent 48%, #fff 49%, #fff 51%, transparent 52%)",
              backgroundSize: "100px 100px",
            }}
          />

          <div className="absolute left-5 top-5 rounded-2xl bg-white p-3 shadow-xl">
            <div className="flex items-center gap-2 text-xs font-black">
              <Navigation size={15} className="text-[#36A667]" />
              Ward 12
            </div>
            <div className="mt-1 text-[10px] text-slate-400">
              24 active issues
            </div>
          </div>

          {filtered.map((issue, index) => (
            <div
              key={issue.id}
              className="absolute"
              style={{
                left: `${15 + ((index * 19) % 70)}%`,
                top: `${18 + ((index * 27) % 62)}%`,
              }}
            >
              <div
                className="flex h-11 w-11 items-center justify-center rounded-full border-4 border-white text-lg shadow-xl"
                style={{ background: issue.color }}
              >
                {issue.icon}
              </div>
            </div>
          ))}

          <div className="absolute bottom-5 left-5 right-5 rounded-2xl bg-white/90 p-4 shadow-xl glass">
            <div className="flex items-center gap-3">
              <div className="flex h-10 w-10 items-center justify-center rounded-xl bg-[#E7F7ED]">
                <LocateFixed size={18} className="text-[#31A967]" />
              </div>
              <div>
                <div className="text-xs font-black">Your location</div>
                <div className="text-[10px] text-slate-400">
                  Near Ward 12, Nagpur
                </div>
              </div>
            </div>
          </div>
        </div>

        <div>
          <div className="rounded-[30px] border border-[#E4EBE6] bg-white p-4 soft-shadow-sm">
            <div className="flex items-center gap-2 rounded-2xl bg-[#F6F8F7] px-4 py-3">
              <Search size={18} className="text-slate-400" />
              <input
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Search issues, places or categories..."
                className="w-full bg-transparent text-sm font-semibold outline-none placeholder:text-slate-400"
              />
            </div>

            <div className="mt-3 flex gap-2 overflow-x-auto pb-1 hide-scrollbar">
              {["All", "Active", "Resolved"].map((item) => (
                <button
                  key={item}
                  onClick={() => setFilter(item)}
                  className={`whitespace-nowrap rounded-xl px-4 py-2 text-xs font-black ${
                    filter === item
                      ? "bg-[#183D2B] text-white"
                      : "bg-slate-50 text-slate-500"
                  }`}
                >
                  {item}
                </button>
              ))}

              <button className="ml-auto flex shrink-0 items-center gap-1 rounded-xl bg-slate-50 px-3 py-2 text-xs font-black text-slate-500">
                <Filter size={13} />
                Filters
              </button>
            </div>
          </div>

          <div className="mt-4 space-y-3">
            {filtered.map((issue) => (
              <div
                key={issue.id}
                className="rounded-[25px] border border-[#E4EBE6] bg-white p-4 transition hover:shadow-lg"
              >
                <div className="flex gap-4">
                  <div
                    className="flex h-14 w-14 shrink-0 items-center justify-center rounded-2xl text-2xl"
                    style={{ background: `${issue.color}15` }}
                  >
                    {issue.icon}
                  </div>

                  <div className="min-w-0 flex-1">
                    <div className="flex flex-wrap items-start justify-between gap-2">
                      <div>
                        <div className="mb-1 text-[10px] font-black uppercase tracking-wider text-slate-400">
                          {issue.id} · {issue.category}
                        </div>
                        <h3 className="font-black">{issue.title}</h3>
                      </div>
                      <StatusBadge status={issue.status} />
                    </div>

                    <p className="mt-2 text-xs leading-5 text-slate-400">
                      {issue.description}
                    </p>

                    <div className="mt-3 flex flex-wrap items-center gap-3 text-[10px] font-bold text-slate-400">
                      <span className="flex items-center gap-1">
                        <MapPin size={12} />
                        {issue.location}
                      </span>
                      <span>•</span>
                      <span>{issue.reported}</span>
                      <span className="ml-auto flex items-center gap-1 text-slate-500">
                        <ThumbsUp size={12} />
                        {issue.votes} supports
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            ))}

            {filtered.length === 0 && (
              <div className="rounded-[30px] border border-dashed border-slate-200 bg-white p-12 text-center">
                <Search className="mx-auto text-slate-300" size={30} />
                <div className="mt-3 font-black">No issues found</div>
                <div className="mt-1 text-xs text-slate-400">
                  Try a different search or filter.
                </div>
              </div>
            )}
          </div>
        </div>
      </div>
    </>
  );
}

/* =========================================================
   MY REPORTS
   ========================================================= */

function MyReports({ issues }: { issues: Issue[] }) {
  return (
    <>
      <div className="mb-6">
        <div className="mb-2 text-xs font-black uppercase tracking-[.15em] text-[#36A667]">
          Citizen dashboard
        </div>
        <h1 className="text-3xl font-black tracking-tight">
          Your reports
        </h1>
        <p className="mt-2 text-sm text-slate-400">
          Keep track of every issue you've reported.
        </p>
      </div>

      <div className="grid gap-6 lg:grid-cols-[.7fr_1.3fr]">
        <div className="rounded-[30px] bg-[#183D2B] p-6 text-white">
          <div className="text-xs font-bold uppercase tracking-[.15em] text-[#7BDDA1]">
            Your activity
          </div>
          <div className="mt-6 text-5xl font-black">17</div>
          <div className="mt-1 text-sm text-white/50">total reports</div>

          <div className="mt-8 space-y-3">
            {[
              ["Resolved", "12", "bg-emerald-400"],
              ["In progress", "3", "bg-violet-400"],
              ["Reported", "2", "bg-orange-400"],
            ].map(([label, value, color]) => (
              <div
                key={label}
                className="flex items-center gap-3 rounded-2xl bg-white/5 p-3"
              >
                <span className={`h-2.5 w-2.5 rounded-full ${color}`} />
                <span className="flex-1 text-xs font-bold text-white/70">
                  {label}
                </span>
                <span className="text-sm font-black">{value}</span>
              </div>
            ))}
          </div>
        </div>

        <div className="space-y-3">
          {issues.map((issue, index) => (
            <div
              key={issue.id}
              className="rounded-[26px] border border-[#E4EBE6] bg-white p-5"
            >
              <div className="flex gap-4">
                <div className="relative flex w-8 flex-col items-center">
                  <div
                    className={`z-10 flex h-8 w-8 items-center justify-center rounded-full ${
                      issue.status === "Resolved"
                        ? "bg-emerald-100 text-emerald-600"
                        : "bg-blue-100 text-blue-600"
                    }`}
                  >
                    {issue.status === "Resolved" ? (
                      <CheckCircle2 size={16} />
                    ) : (
                      <Clock3 size={16} />
                    )}
                  </div>
                  {index !== issues.length - 1 && (
                    <div className="h-full w-px bg-slate-100" />
                  )}
                </div>

                <div className="flex-1">
                  <div className="flex flex-wrap items-start justify-between gap-2">
                    <div>
                      <div className="text-[10px] font-black uppercase tracking-wider text-slate-400">
                        {issue.id}
                      </div>
                      <h3 className="mt-1 font-black">{issue.title}</h3>
                    </div>
                    <StatusBadge status={issue.status} />
                  </div>

                  <div className="mt-3 rounded-2xl bg-[#F8FAF8] p-3">
                    <div className="flex items-center gap-2 text-[10px] font-black text-slate-500">
                      <Landmark size={13} className="text-[#35A765]" />
                      Municipal update
                    </div>
                    <p className="mt-1 text-[11px] leading-5 text-slate-400">
                      {issue.status === "Resolved"
                        ? "The reported issue has been inspected and resolved."
                        : "Your report has been received and is being processed by the relevant department."}
                    </p>
                  </div>
                </div>
              </div>
            </div>
          ))}
        </div>
      </div>
    </>
  );
}

/* =========================================================
   COMMUNITY
   ========================================================= */

function CommunityPage({ issues }: { issues: Issue[] }) {
  return (
    <>
      <div className="mb-6">
        <div className="mb-2 text-xs font-black uppercase tracking-[.15em] text-[#36A667]">
          Community
        </div>
        <h1 className="text-3xl font-black tracking-tight">
          Citizens helping citizens
        </h1>
        <p className="mt-2 max-w-xl text-sm text-slate-400">
          Support issues that matter to your neighborhood and celebrate the
          people making a difference.
        </p>
      </div>

      <div className="grid gap-6 lg:grid-cols-[1.2fr_.8fr]">
        <div className="rounded-[30px] border border-[#E4EBE6] bg-white p-6">
          <div className="flex items-center justify-between">
            <div>
              <h2 className="font-black">Trending issues</h2>
              <p className="mt-1 text-xs text-slate-400">
                Most supported this week
              </p>
            </div>
            <TrendingUp size={20} className="text-[#35A765]" />
          </div>

          <div className="mt-5 space-y-3">
            {[...issues]
              .sort((a, b) => b.votes - a.votes)
              .map((issue, index) => (
                <div
                  key={issue.id}
                  className="flex items-center gap-4 rounded-2xl bg-[#F8FAF8] p-4"
                >
                  <div className="w-5 text-center text-xs font-black text-slate-400">
                    0{index + 1}
                  </div>
                  <div
                    className="flex h-11 w-11 items-center justify-center rounded-xl text-xl"
                    style={{ background: `${issue.color}15` }}
                  >
                    {issue.icon}
                  </div>
                  <div className="min-w-0 flex-1">
                    <div className="truncate text-xs font-black">
                      {issue.title}
                    </div>
                    <div className="mt-1 text-[10px] text-slate-400">
                      {issue.location}
                    </div>
                  </div>
                  <div className="flex items-center gap-1 rounded-xl bg-white px-3 py-2 text-xs font-black text-slate-500">
                    <ThumbsUp size={12} />
                    {issue.votes}
                  </div>
                </div>
              ))}
          </div>
        </div>

        <div className="rounded-[30px] bg-[#FFF8ED] p-6">
          <div className="flex h-12 w-12 items-center justify-center rounded-2xl bg-white text-2xl shadow-sm">
            🏅
          </div>
          <h2 className="mt-5 text-2xl font-black">
            Citizen of the week
          </h2>
          <p className="mt-2 text-sm leading-6 text-slate-500">
            Meet community members whose reports and support are helping
            improve everyday life.
          </p>

          <div className="mt-6 rounded-[24px] bg-white p-4 shadow-sm">
            <div className="flex items-center gap-3">
              <div className="flex h-12 w-12 items-center justify-center rounded-full bg-[#D8EFE1] font-black text-[#238851]">
                RP
              </div>
              <div>
                <div className="text-sm font-black">Riya Patil</div>
                <div className="text-[10px] text-slate-400">
                  38 helpful reports
                </div>
              </div>
              <Star
                size={17}
                className="ml-auto fill-[#F5B82E] text-[#F5B82E]"
              />
            </div>
          </div>

          <div className="mt-4 grid grid-cols-2 gap-3">
            <div className="rounded-2xl bg-white p-4">
              <div className="text-xl font-black">1.2K</div>
              <div className="mt-1 text-[10px] text-slate-400">
                Supports given
              </div>
            </div>
            <div className="rounded-2xl bg-white p-4">
              <div className="text-xl font-black">94%</div>
              <div className="mt-1 text-[10px] text-slate-400">
                Resolution rate
              </div>
            </div>
          </div>
        </div>
      </div>
    </>
  );
}

/* =========================================================
   REPORT MODAL
   ========================================================= */

function ReportModal({
  t,
  onClose,
  onSubmit,
}: {
  t: any;
  onClose: () => void;
  onSubmit: (issue: Issue) => void;
}) {
  const [category, setCategory] = useState("Roads");
  const [title, setTitle] = useState("");
  const [description, setDescription] = useState("");
  const [location, setLocation] = useState("Ward 12, Nagpur");
  const [submitted, setSubmitted] = useState(false);

  const submit = () => {
    if (!title.trim()) return;

    const cat = categories.find((c) => c.name === category);

    setSubmitted(true);

    setTimeout(() => {
      onSubmit({
        id: `CIV-${Math.floor(1000 + Math.random() * 8999)}`,
        title,
        category,
        description:
          description ||
          "Citizen reported issue submitted through Civicly.",
        location,
        status: "Reported",
        priority: "Medium",
        reported: "just now",
        votes: 1,
        color: cat?.color || "#48B17B",
        icon: cat?.icon || "📍",
      });
    }, 900);
  };

  return (
    <div className="fixed inset-0 z-[100] flex items-end justify-center bg-[#102219]/45 p-0 backdrop-blur-sm sm:items-center sm:p-5">
      <div className="max-h-[94vh] w-full max-w-2xl overflow-auto rounded-t-[32px] bg-white p-5 shadow-2xl sm:rounded-[32px] sm:p-7">
        <div className="flex items-center justify-between">
          <div>
            <div className="mb-1 text-xs font-black uppercase tracking-[.15em] text-[#35A765]">
              New civic report
            </div>
            <h2 className="text-2xl font-black tracking-tight">
              {t.issueTitle}
            </h2>
          </div>

          <button
            onClick={onClose}
            className="rounded-xl bg-slate-100 p-2.5 text-slate-500 hover:bg-slate-200"
          >
            <X size={18} />
          </button>
        </div>

        {submitted ? (
          <div className="py-16 text-center">
            <div className="mx-auto flex h-20 w-20 items-center justify-center rounded-full bg-[#E5F8EC]">
              <CheckCircle2 size={38} className="text-[#2EAD62]" />
            </div>
            <h3 className="mt-5 text-2xl font-black">
              Report submitted!
            </h3>
            <p className="mx-auto mt-2 max-w-sm text-sm leading-6 text-slate-400">
              Thanks for helping improve your city. Your report has been added
              to the civic network.
            </p>
          </div>
        ) : (
          <>
            <div className="mt-7">
              <label className="mb-2 block text-xs font-black">
                Issue category
              </label>

              <div className="grid grid-cols-3 gap-2 sm:grid-cols-6">
                {categories.map((cat) => (
                  <button
                    key={cat.name}
                    onClick={() => setCategory(cat.name)}
                    className={`rounded-2xl border p-3 transition ${
                      category === cat.name
                        ? "border-[#83D2A2] bg-[#ECF9F1]"
                        : "border-slate-100 bg-slate-50"
                    }`}
                  >
                    <div className="text-xl">{cat.icon}</div>
                    <div className="mt-1 truncate text-[9px] font-black">
                      {cat.name}
                    </div>
                  </button>
                ))}
              </div>
            </div>

            <div className="mt-5">
              <label className="mb-2 block text-xs font-black">
                Title
              </label>
              <input
                value={title}
                onChange={(e) => setTitle(e.target.value)}
                placeholder="e.g. Large pothole near school"
                className="w-full rounded-2xl border border-slate-200 bg-slate-50 px-4 py-3.5 text-sm font-semibold outline-none transition focus:border-[#6DCE93] focus:bg-white"
              />
            </div>

            <div className="mt-5">
              <label className="mb-2 block text-xs font-black">
                {t.description}
              </label>
              <textarea
                value={description}
                onChange={(e) => setDescription(e.target.value)}
                rows={4}
                placeholder="Tell us what happened, where it is and why it needs attention..."
                className="w-full resize-none rounded-2xl border border-slate-200 bg-slate-50 px-4 py-3.5 text-sm font-semibold outline-none transition focus:border-[#6DCE93] focus:bg-white"
              />
            </div>

            <div className="mt-5">
              <label className="mb-2 block text-xs font-black">
                {t.location}
              </label>

              <div className="flex gap-2">
                <div className="flex flex-1 items-center gap-2 rounded-2xl border border-slate-200 bg-slate-50 px-4">
                  <MapPin size={16} className="text-[#38A968]" />
                  <input
                    value={location}
                    onChange={(e) => setLocation(e.target.value)}
                    className="w-full bg-transparent py-3.5 text-sm font-semibold outline-none"
                  />
                </div>

                <button className="flex items-center gap-2 rounded-2xl bg-[#E7F7ED] px-4 text-xs font-black text-[#248C51]">
                  <LocateFixed size={15} />
                  <span className="hidden sm:block">
                    {t.useLocation}
                  </span>
                </button>
              </div>
            </div>

            <div className="mt-5">
              <label className="mb-2 block text-xs font-black">
                {t.upload}
              </label>

              <div className="grid grid-cols-2 gap-3 sm:grid-cols-3">
                <button className="flex h-28 flex-col items-center justify-center rounded-2xl border-2 border-dashed border-[#CFE7D7] bg-[#F6FBF7] text-[#35A765]">
                  <ImagePlus size={22} />
                  <span className="mt-2 text-[10px] font-black">
                    Upload photo
                  </span>
                </button>

                <button className="flex h-28 flex-col items-center justify-center rounded-2xl border-2 border-dashed border-slate-200 bg-slate-50 text-slate-400">
                  <Camera size={22} />
                  <span className="mt-2 text-[10px] font-black">
                    Take photo
                  </span>
                </button>

                <div className="hidden h-28 items-center justify-center rounded-2xl bg-[#EEF2F0] text-4xl sm:flex">
                  🏙️
                </div>
              </div>
            </div>

            <div className="mt-6 flex items-start gap-3 rounded-2xl bg-[#F6FAF7] p-4">
              <Info size={17} className="mt-0.5 shrink-0 text-[#38A968]" />
              <p className="text-[10px] leading-5 text-slate-400">
                Avoid sharing personal information in photos. Your report may
                be visible to other citizens to help coordinate community
                action.
              </p>
            </div>

            <div className="mt-6 flex flex-col-reverse gap-2 sm:flex-row sm:justify-end">
              <button
                onClick={onClose}
                className="rounded-2xl px-5 py-3 text-sm font-black text-slate-500 hover:bg-slate-50"
              >
                Cancel
              </button>

              <button
                onClick={submit}
                disabled={!title.trim()}
                className="flex items-center justify-center gap-2 rounded-2xl bg-[#183D2B] px-6 py-3.5 text-sm font-black text-white shadow-lg shadow-emerald-900/10 disabled:cursor-not-allowed disabled:opacity-40"
              >
                <Send size={16} />
                {t.submit}
              </button>
            </div>
          </>
        )}
      </div>
    </div>
  );
}# Citizn
