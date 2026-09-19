import React, { useState, useEffect, useRef } from 'react';
import { 
  Shield, Search, Filter, Clock, BookOpen, Archive, User, 
  Terminal, Flame, Coins, FileText, CheckCircle2, MapPin, 
  AlertCircle, Calendar, ChevronRight, HelpCircle, ArrowRight,
  TrendingUp, Award, ThumbsUp, Lock, RefreshCw, Send, Sparkles,
  Globe, Compass, Crosshair, Eye, Coffee, Building2, Key, Layers, Users
} from 'lucide-react';

// Comprehensive Master Historical Database mapped across exact coordinates (calculated from ~690 BCE birth)
const masterTimeline = [
  {
    id: "babylon",
    year: "590 BCE",
    location: "Babylon (Modern Iraq) & Persia",
    lat: 32.5422,
    lng: 44.4211,
    mapPos: { x: 60.8, y: 39.5 },
    figure: "Nabu-apla-iddina & Chief Royal Architect",
    antic: "Spotted drainage subsidence on the second terrace. Took a tile from the Hanging Gardens, walked east into Persia, and turned back three months later at age 100 simply because he felt like it.",
    got: "Lapis Lazuli Paving Tile from Hanging Gardens",
    advice: "Told them that water always finds the weakness and recommended redistributing soil weight.",
    sassyNote: "Turning around when you feel like it is one of the privileges of not being dead.",
    era: "classical"
  },
  {
    id: "suntzu",
    year: "512 BCE",
    location: "Wu State (Modern Suzhou, China)",
    lat: 31.2989,
    lng: 120.5853,
    mapPos: { x: 79.5, y: 41.5 },
    figure: "Sun Tzu (Sun Wu)",
    antic: "Posed as a passing itinerant warrior, debated Sun Tzu on the necessity of deception vs. sheer stubborn persistence, and complained loudly about walking across Asia.",
    got: "Annotated Bamboo Scrolls of 'The Art of War'",
    advice: "Warned Sun Tzu that strict discipline works for armies, but individual human stubbornness always outlasts military doctrine.",
    sassyNote: "Sun Tzu wrote down my complaints about bad boots and supply lines and called it strategic wisdom. I just wanted better footwear.",
    era: "classical"
  },
  {
    id: "thermopylae",
    year: "480 BCE",
    location: "Thermopylae, Greece",
    lat: 38.7966,
    lng: 22.5361,
    mapPos: { x: 53.8, y: 37.2 },
    figure: "King Leonidas of Sparta",
    antic: "Had not planned to be here. Fought on the Spartan right flank as an unnamed foreigner; personally caught a spear aimed at Leonidas's back.",
    got: "Leonidas's Spartan Xiphos Sword",
    advice: "Warned generals to send the fleet to Artemisium and hold the pass simultaneously. Told them never to execute a strategy halfway.",
    sassyNote: "Preserved in the vault. Please don't touch, still sharp. Carries the weight of 2,700 years of survival guilt.",
    era: "classical"
  },
  {
    id: "macedon",
    year: "336 BCE",
    location: "Pella / Gordium (Macedon & Phrygia)",
    lat: 40.7558,
    lng: 22.5186,
    mapPos: { x: 54.2, y: 35.8 },
    figure: "Alexander the Great",
    antic: "Tamed Bucephalus by turning him toward the sun. Subtly goaded Alexander into slicing the Gordian Knot.",
    got: "Gordian Knot Fragments & Signet",
    advice: "Warned Alexander at age 22 that conquest objectives naturally expand into fatal megalomania.",
    sassyNote: "He cut the knot after I suggested the legend did not specify methodology. Still annoyed I didn't think of it first.",
    era: "classical"
  },
  {
    id: "alexandria",
    year: "48 BCE",
    location: "Alexandria, Egypt",
    lat: 31.2001,
    lng: 29.9187,
    mapPos: { x: 56.5, y: 42.5 },
    figure: "Queen Cleopatra VII",
    antic: "Came to Alexandria specifically for the library. Hid 47 scrolls in the back of the natural philosophy collection behind a moved shelf.",
    got: "47 Papyrus Scrolls & Ptolemaic Sapphire Ring",
    advice: "Served as Cleopatra's secret thinking partner. Told her Caesar's brilliance was genuine fast-thinking but would become fatal overconfidence.",
    sassyNote: "Borrowed these from the Library. Wrote return date in own handwriting that passed 2,000 years ago. Oops.",
    era: "classical"
  },
  {
    id: "rubicon",
    year: "49-44 BCE",
    location: "Rome & Rubicon River, Italy",
    lat: 41.9028,
    lng: 12.4964,
    mapPos: { x: 50.8, y: 35.8 },
    figure: "Julius Caesar",
    antic: "Found Caesar unexpectedly funny—cracking complex double-conversational jokes. Warned Caesar specifically about 23 senators with knives at Pompey's theatre.",
    got: "Julius Caesar Silver Denarius Coin",
    advice: "Warned Caesar directly about the plot. Caesar thanked him for the intel and accepted the situation anyway.",
    sassyNote: "Julius gave me the coin personally. Kept it in my coat pocket for sentimental value ever since.",
    era: "classical"
  },
  {
    id: "camelot",
    year: "497 CE",
    location: "Somerset, Britain",
    lat: 51.0500,
    lng: -2.7500,
    mapPos: { x: 45.8, y: 28.5 },
    figure: "King Arthur & Merlin",
    antic: "Registered as 'Sir Percival'. Found Arthur's steel sword in a lake after Arthur died on an island.",
    got: "The Excalibur Sword (Steel with gold inlay)",
    advice: "Warned Arthur twice about Mordred being a structural threat.",
    sassyNote: "Insurance form quote: 'Found this in a lake after Arthur died. Don't ask. Not saying it's Excalibur but not saying it's NOT Excalibur either.' British Museum keeps asking; answer is NO.",
    era: "medieval"
  },
  {
    id: "vinland",
    year: "999-1000 CE",
    location: "Norway & Vinland (L'Anse aux Meadows)",
    lat: 51.5951,
    lng: -55.5312,
    mapPos: { x: 28.5, y: 28.0 },
    figure: "Leif Eriksson & Gunnar the Viking",
    antic: "Challenged 30 Vikings to a longhouse drinking contest in Norway and won, then joined Leif Eriksson's voyage across the Atlantic.",
    got: "L'Anse aux Meadows Wild Grape Wine Flask",
    advice: "Gunnar lost fairly and got a good story out of it. Told Leif to look for timber in Labrador.",
    sassyNote: "Good mead. Great voyage. New continent. 10/10 would sail west again.",
    era: "medieval"
  },
  {
    id: "crusades",
    year: "1096-1150 CE",
    location: "Antioch & Damascus (Outremer)",
    lat: 36.2021,
    lng: 36.1601,
    mapPos: { x: 58.2, y: 38.2 },
    figure: "Hugh de Payens & Everard des Barres",
    antic: "Negotiated custom rank 'Summus Magister Maximus' (Petros of Antioch) in Vatican records because he refused standard vows after taking more oaths than he could remember.",
    got: "Templar Seal & Damascus Steel Dagger",
    advice: "Told Vatican officials that written rank definitions have a way of becoming arguments.",
    sassyNote: "Vatican margin note by Pope Honorius II in 1128 AD: 'File under: unusual.'",
    era: "medieval"
  },
  {
    id: "florence",
    year: "1494-1506 CE",
    location: "Florence & Amboise, Italy/France",
    lat: 43.7696,
    lng: 11.2558,
    mapPos: { x: 50.2, y: 34.2 },
    figure: "Leonardo da Vinci",
    antic: "Posed for the 'Vitruvian Man' drawing for 2 hours and 26 minutes. Kept complaining his arms hurt while Leonardo told him to hush.",
    got: "Leonardo's Original Personal Leather Notebook",
    advice: "Pointed out Leonardo's flying machine sketches were physically wrong in three specific places, but praised his structural mechanics.",
    sassyNote: "I've been alive for 2,200 years and was just told to hush by a man with ink on his hands who is wrong about flying machines in 3 places and right about everything else.",
    era: "early-modern"
  },
  {
    id: "elizabethan",
    year: "1583 CE",
    location: "London, England",
    lat: 51.5074,
    lng: -0.1278,
    mapPos: { x: 46.2, y: 28.2 },
    figure: "Queen Elizabeth I & Sir Francis Walsingham",
    antic: "Exposed a Spanish plot in Cheapside. Received knight status (Sir Piers) creating a 440+ year legal loophole.",
    got: "Formal Knighthood & Whitmore Sterling Founding Framework",
    advice: "Advised Walsingham on deep cover networks.",
    sassyNote: "Nigel Pierce to US Intelligence in 2024: 'We have 450 years of institutional memory. You Americans have 80 years and keep forgetting.'",
    era: "early-modern"
  },
  {
    id: "shakespeare",
    year: "1592 CE",
    location: "London, England (Mermaid Tavern)",
    lat: 51.5074,
    lng: -0.1278,
    mapPos: { x: 46.5, y: 27.8 },
    figure: "William Shakespeare",
    antic: "Bought good wine at Mermaid Tavern while reading ancient Greek. Told Will about a Danish prince (Hamlet) and Italian lovers (Juliet lived to 74 in Mantua!).",
    got: "Seven Original Playscripts (Hamlet, Romeo & Juliet, etc.)",
    advice: "Furious that Will killed Juliet for dramatic effect when she actually lived to 74 in Mantua with a herb garden and 4 grandkids. Demanded a cut of royalties.",
    sassyNote: "Will laughed when I demanded royalties. He wrote plays, not histories. Still no royalties after 430 years.",
    era: "early-modern"
  },
  {
    id: "whitmore1792",
    year: "1792-1803 CE",
    location: "London & Geneva, Switzerland",
    lat: 46.2044,
    lng: 6.1432,
    mapPos: { x: 48.5, y: 31.8 },
    figure: "Jonathan Whitmore III & Edmund Sterling",
    antic: "Wrote a letter to his lawyers complaining about tripping over Charlemagne's crown on the floor and having 47 Alexandria scrolls wrapped in Paris.",
    got: "The 2,500 SQM Swiss Vault Complex",
    advice: "Commissioned the ultimate private vault. Stored the sealed Munich crate from Jerusalem (33 AD) without opening it.",
    sassyNote: "Jonathan Whitmore IV handed over symbolic key in 1803. Vault size: size of a small warehouse. Value: Incalculable.",
    era: "late-modern"
  },
  {
    id: "croissant",
    year: "1995 CE",
    location: "Paris, France",
    lat: 48.8566,
    lng: 2.3522,
    mapPos: { x: 47.2, y: 30.5 },
    figure: "French Intelligence & The Illuminati",
    antic: "Arrested while eating a warm croissant. Triggered 845-year-old Templar rank 'Summus Magister Maximus' to compel extraction in 43 mins.",
    got: "Almond Pastry Recommendation & French Fruit Basket",
    advice: "Told French interrogators to move him to a nicer room with coffee. French DGSE later sent a fruit basket to CIA as a joke.",
    sassyNote: "Faster than Echelon. Went back next morning for the almond croissant.",
    era: "contemporary"
  }
];

// Expanded Artifacts Vault Directory based on Legal Filings & Whitmore Sterling 1792 Letters
const VAULT_ITEMS = [
  { id: "0001", name: "Thermopylae Xiphos", era: "480 BCE Greece", val: "€20M", note: "Used at Thermopylae. Kept for sentimental reasons. Please don't touch, still sharp.", category: "weapons" },
  { id: "0012", name: "Sun Tzu's Annotated Bamboo Scrolls", era: "512 BCE China", val: "€95M", note: "Original bamboo slips containing 'The Art of War' with margin notes by Perseus criticizing footwear options.", category: "documents" },
  { id: "0043", name: "The Excalibur Sword", era: "497 CE Britain", val: "€75M", note: "Insurance entry: 'Found this in a lake after Arthur died. Don't ask. Not saying it's Excalibur but not saying it's NOT Excalibur either.' British Museum keeps begging.", category: "weapons" },
  { id: "0102", name: "Sealed Jerusalem Crate (Munich Crate)", era: "Jerusalem (33 AD)", val: "Incalculable", note: "Acquired in Jerusalem ~33 AD. Kept in a crate in Munich for centuries. Moved to Swiss Vault Room 7 in 1803. UNOPENED by Whitmore Sterling request.", category: "religious" },
  { id: "0234", name: "Mesopotamian Bronze Dagger", era: "Babylonian Empire", val: "€35M", note: "Stored on a table in Edinburgh in 1792 next to Shakespeare manuscripts before moving to Swiss Vault.", category: "weapons" },
  { id: "0456", name: "Charlemagne's Imperial Crown", era: "800 CE Carolingian", val: "€60M", note: "Perseus tripped over this on the floor of his Paris property in 1792 before Whitmore Sterling built the vault. Wore it once to be polite. Heavy.", category: "regalia" },
  { id: "0523", name: "Holy Roman Empire Scepter", era: "Holy Roman Empire", val: "€30M", note: "Payment for services rendered. Formally cataloged in Section 4 (Regalia) in 1803.", category: "regalia" },
  { id: "0687", name: "Cleopatra's Royal Sapphire Ring", era: "48 BCE Egypt", val: "€50M", note: "Given by Cleopatra in Alexandria. Perseus carried it in his coat pocket for decades before lawyers forced him to put it in Vault Jewelry Housing.", category: "regalia" },
  { id: "1087", name: "Judean Clay Cup (The Holy Grail?)", era: "Jerusalem (33 AD)", val: "Incalculable", note: "Legal entry 1792: 'What might be a Holy Grail but I'm genuinely not certain.' Stored under climate control. Vatican extremely curious.", category: "religious" },
  { id: "1205", name: "Byzantine Icon from Vienna", era: "Constantinople / Vienna", val: "€15M", note: "Appeared in Perseus's Vienna property without explanation. 'Possible I've been gifted things in my sleep.' Collected by Whitmore Sterling 1793.", category: "religious" },
  { id: "2034", name: "47 Library of Alexandria Papyrus Scrolls", era: "300-48 BCE Egypt", val: "Incalculable", note: "Hidden behind natural philosophy shelf. Wrapped in Paris until 1792. Now in Vault Archival Pod #1 with strict humidity control.", category: "documents" },
  { id: "2156", name: "Leonardo da Vinci's Personal Leather Notebook", era: "1506 CE Florence", val: "€150M+", note: "Thick notebook with Vitruvian Man sketch on cover. Contains flight sketches wrong in 3 places and endless geometry ramblings.", category: "documents" },
  { id: "2267", name: "Shakespeare's Seven Original Playscripts", era: "1592 CE London", val: "€200M+", note: "Originals of Hamlet, Romeo & Juliet, etc. Moved from London to Edinburgh due to dampness, then to Vault Pod #2 in 1803.", category: "documents" },
  { id: "3001", name: "Parthenon Marble Fragments (3 Pieces)", era: "432 BCE Athens", val: "€40M", note: "Stored in Edinburgh for 400 years with intention to return to Greece. Legal note: 'Greece can ask properly if they want them back.'", category: "miscellaneous" },
  { id: "3012", name: "Hanging Gardens Lapis Lazuli Tile", era: "590 BCE Babylon", val: "€25M", note: "Paved drainage tile taken from Babylon when Perseus turned around in Persia at age 100.", category: "miscellaneous" },
  { id: "4001", name: "Julius Caesar Silver Denarius Coin", era: "49 BCE Rome", val: "€10M", note: "One of 2,000+ ancient coins in Vault Coin Storage. Sentimental favorite given personally by Caesar.", category: "miscellaneous" }
];

// Documented Vault Visitors (Past 50 Years Public Banking Records)
const VAULT_VISITORS = [
  { date: "Quarterly (Clockwork)", visitor: "Perseus Jackson ('P. Jackson')", purpose: "Routine inspection & coat pocket item drop-off", clearance: "OMEGA-1" },
  { date: "November 2018", visitor: "The Sovereign Pontiff (Vatican)", purpose: "Coffee with Perseus & inspection of Judean Cup / Munich Crate", clearance: "Holy See Direct" },
  { date: "June 2012", visitor: "Director of the Louvre Museum", purpose: "Begging for da Vinci Notebook & Excalibur (Request Denied)", clearance: "UNESCO-Alpha" },
  { date: "March 2005", visitor: "Board of Trustees, British Museum", purpose: "Formal inquiry on Parthenon fragments & Arthurian Steel (Denied)", clearance: "State Royal" },
  { date: "September 1998", visitor: "Director General, UNESCO", purpose: "Cultural heritage preservation audit (2,500 SQM certified)", clearance: "UN-Class 1" },
  { date: "August 1984", visitor: "Head of State (Confidential / Classified)", purpose: "Private diplomatic consultation", clearance: "Echelon Red" }
];

export default function App() {
  const [activeTab, setActiveTab] = useState('dashboard');
  const [selectedEra, setSelectedEra] = useState('all');
  const [timelineFilter, setTimelineFilter] = useState('all');
  const [vaultSearch, setVaultSearch] = useState('');
  const [vaultCategory, setVaultCategory] = useState('all');
  
  // Interactive Map State
  const [selectedMapPin, setSelectedMapPin] = useState(masterTimeline[0]);
  const [mapEraFilter, setMapEraFilter] = useState('all');

  // Terminal state
  const [terminalInput, setTerminalInput] = useState('');
  const [terminalLogs, setTerminalLogs] = useState([
    "PROJECT OMEGA ACTIVE SECURE TERMINAL...",
    "AUTHENTICATION REQUIRED. ENTER AUTHORIZATION PHRASE TO TRIGGER GHOST PROTOCOL EXTRACTION."
  ]);
  const [isGhostActive, setIsGhostActive] = useState(false);
  const terminalEndRef = useRef(null);

  // Auto-scroll terminal
  useEffect(() => {
    if (terminalEndRef.current) {
      terminalEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [terminalLogs]);

  // Handle terminal commands
  const handleTerminalSubmit = (e) => {
    e.preventDefault();
    const cleanInput = terminalInput.trim();
    if (!cleanInput) return;

    let newLogs = [...terminalLogs, `> ${cleanInput}`];

    if (cleanInput.toLowerCase() === 'november-hotel-seven-seven-three-nine') {
      setIsGhostActive(true);
      newLogs = [
        ...newLogs,
        "⚠️ [CRITICAL ALERT] AUTHORIZATION CODE ACCEPTED.",
        "🔓 OMEGA-LEVEL SECURE PROTOCOL UNLOCKED.",
        "🚁 ACTIVATING GHOST PROTOCOL EXTRACTION TEAMS...",
        "📍 DEPLOYING GHOST TEAMS FROM FORT BRAGG...",
        "⏱️ ESTIMATED TIME OF ARRIVAL: 16 MINUTES, 43 SECONDS.",
        "📡 GLOBAL AGENCY FLAGS: RED STATUS INITIATED.",
        "📞 INITIATING GHOST TEAM COMMS... COMMANDER MORRISON LOGGED IN.",
        "Morrison: 'Hold tight, Mr. Jackson. We are on the way. Don't sign anything, don't say anything, and make sure you have your book.'"
      ];
    } else if (cleanInput.toLowerCase() === 'clear') {
      newLogs = ["PROJECT OMEGA ACTIVE SECURE TERMINAL..."];
      setIsGhostActive(false);
    } else if (cleanInput.toLowerCase() === 'help') {
      newLogs = [
        ...newLogs,
        "Available Commands:",
        "  november-hotel-seven-seven-three-nine - Trigger Echelon Extraction",
        "  clear                                - Clear terminal logs",
        "  help                                 - Show this menu"
      ];
    } else {
      newLogs = [
        ...newLogs,
        `❌ COMMAND OR AUTHORIZATION CODE '${cleanInput}' NOT RECOGNIZED.`,
        "Type 'help' for options or search the classified logs for the passcode."
      ];
    }

    setTerminalLogs(newLogs);
    setTerminalInput('');
  };

  // Filter timeline based on era and type filters
  const filteredTimeline = masterTimeline.filter(item => {
    const eraMatch = selectedEra === 'all' || item.era === selectedEra;
    
    let typeMatch = true;
    if (timelineFilter === 'intervention') typeMatch = !!item.antic;
    if (timelineFilter === 'advice') typeMatch = !!item.advice;
    if (timelineFilter === 'artifact') typeMatch = !!item.got;
    
    return eraMatch && typeMatch;
  });

  // Filter map pins
  const filteredMapPins = masterTimeline.filter(item => {
    return mapEraFilter === 'all' || item.era === mapEraFilter;
  });

  // Filter vault items
  const filteredVault = VAULT_ITEMS.filter(item => {
    const categoryMatch = vaultCategory === 'all' || item.category === vaultCategory;
    const searchMatch = item.name.toLowerCase().includes(vaultSearch.toLowerCase()) ||
                        item.era.toLowerCase().includes(vaultSearch.toLowerCase()) ||
                        item.note.toLowerCase().includes(vaultSearch.toLowerCase());
    return categoryMatch && searchMatch;
  });

  return (
    <div className="min-h-screen bg-slate-950 text-slate-100 flex flex-col font-sans selection:bg-teal-500 selection:text-slate-950">
      
      {/* Top Banner Warning */}
      <div className="bg-gradient-to-r from-amber-600 via-amber-500 to-amber-600 text-slate-950 py-1.5 px-4 text-center text-xs font-black tracking-wider uppercase flex items-center justify-center gap-2 shadow-lg">
        <AlertCircle size={14} className="animate-pulse" />
        <span>Classified Information - OMEGA Clearance Only - Protected by Whitmore, Sterling & Associates (1792 Framework)</span>
      </div>

      {/* Main Header / Navigation */}
      <header className="border-b border-slate-800 bg-slate-900/60 backdrop-blur sticky top-0 z-30 px-6 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
        <div className="flex items-center gap-3">
          <div className="p-2.5 bg-teal-950 text-teal-400 rounded-lg border border-teal-800/50 shadow-lg shadow-teal-950/40">
            <Shield size={24} className="animate-pulse" />
          </div>
          <div>
            <h1 className="text-xl font-bold tracking-tight text-white flex items-center gap-2">
              PROJECT OMEGA <span className="text-xs bg-slate-800 text-slate-400 px-2 py-0.5 rounded font-mono">v4.30-EXP</span>
            </h1>
            <p className="text-xs text-slate-400 font-mono">Historical Dossier & Reconstructed Timeline: Perseus Jackson</p>
          </div>
        </div>

        {/* Global Metric Header Widget */}
        <div className="flex items-center gap-6 bg-slate-950/80 px-4 py-2 rounded-lg border border-slate-800 text-xs font-mono">
          <div className="flex flex-col">
            <span className="text-[10px] text-slate-500 uppercase">SWISS VAULT SIZE</span>
            <span className="text-teal-400 font-bold flex items-center gap-1">
              <Building2 size={12} /> 2,500 m²
            </span>
          </div>
          <div className="w-px h-8 bg-slate-800" />
          <div className="flex flex-col">
            <span className="text-[10px] text-slate-500 uppercase">LEGAL PROTECTION</span>
            <span className="text-amber-400 font-bold flex items-center gap-1">
              <Lock size={12} /> Whitmore (1792)
            </span>
          </div>
          <div className="w-px h-8 bg-slate-800" />
          <div className="flex flex-col">
            <span className="text-[10px] text-slate-500 uppercase">RECONSTRUCTED AGE</span>
            <span className="text-blue-400 font-bold">2,716 Yrs (b. ~690 BCE)</span>
          </div>
        </div>
      </header>

      {/* Workspace Area: Sidebar + Dashboard */}
      <div className="flex-1 flex flex-col md:flex-row overflow-hidden">
        
        {/* Navigation Sidebar */}
        <aside className="w-full md:w-64 bg-slate-900/40 border-r border-slate-800 flex-shrink-0 p-4 space-y-2">
          <p className="text-[10px] text-slate-500 font-mono tracking-widest uppercase mb-4 px-2">INTELLIGENCE MODULES</p>
          
          <button 
            onClick={() => setActiveTab('dashboard')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'dashboard' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <BookOpen size={16} />
            <span>Historian's Dossier</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('map')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'map' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <Globe size={16} />
            <span>Global Tactical Map</span>
          </button>

          <button 
            onClick={() => setActiveTab('timeline')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'timeline' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <Clock size={16} />
            <span>Interactive Timeline</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('vault')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'vault' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <Archive size={16} />
            <span>Swiss Vault & Visitors</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('forum')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'forum' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <FileText size={16} />
            <span>Leaked Forums & Memos</span>
          </button>
          
          <button 
            onClick={() => setActiveTab('terminal')} 
            className={`w-full text-sm font-medium transition-all duration-150 p-2.5 rounded-lg flex items-center gap-3 ${activeTab === 'terminal' ? 'bg-teal-500 text-slate-950 font-bold' : 'hover:bg-slate-800 text-slate-400 hover:text-white'}`}
          >
            <Terminal size={16} />
            <span>Echelon Active Console</span>
          </button>

          <div className="pt-6 mt-6 border-t border-slate-800/80">
            <div className="p-3 bg-slate-950/60 rounded-lg border border-slate-800/50">
              <span className="text-[10px] text-slate-500 font-mono uppercase block mb-1">Legal Counsel Firm</span>
              <div className="space-y-1 text-xs">
                <p className="font-bold text-amber-400">Whitmore, Sterling & Assoc.</p>
                <p className="text-[10px] text-slate-400 font-mono">London, Est. 1678</p>
                <p className="text-[10px] text-slate-500 italic mt-1">"450 years of institutional memory."</p>
              </div>
            </div>
          </div>
        </aside>

        {/* Work Area */}
        <main className="flex-1 overflow-y-auto p-6 md:p-8 space-y-8">
          
          {/* TAB 1: HISTORIAN'S DASHBOARD */}
          {activeTab === 'dashboard' && (
            <div className="space-y-8 animate-fadeIn">
              
              {/* Introduction Card */}
              <div className="p-6 md:p-8 bg-gradient-to-br from-slate-900 to-slate-950 rounded-2xl border border-slate-800 relative overflow-hidden shadow-2xl">
                <div className="absolute top-0 right-0 w-96 h-96 bg-teal-500/5 rounded-full blur-3xl pointer-events-none"></div>
                <div className="absolute bottom-0 left-0 w-96 h-96 bg-amber-500/5 rounded-full blur-3xl pointer-events-none"></div>
                
                <div className="max-w-3xl space-y-4">
                  <div className="inline-flex items-center gap-1.5 px-2.5 py-1 rounded bg-teal-950/80 text-teal-400 border border-teal-800/40 text-[10px] font-mono tracking-widest uppercase">
                    <Sparkles size={10} /> PROJECT OVERVIEW
                  </div>
                  <h2 className="text-3xl font-extrabold tracking-tight text-white md:text-4xl">
                    The Reconstructed Memoirs & Swiss Vault Ledger
                  </h2>
                  <p className="text-slate-300 leading-relaxed text-sm md:text-base">
                    For over <strong>2,700 years</strong> (born ~690 BCE; ~100 years old during the Hanging Gardens of Babylon in 590 BCE), Perseus Jackson has acquired, cataloged, and occasionally misplaced some of the most extraordinary treasures in human history—from Sun Tzu's bamboo slips in China to 47 Library of Alexandria scrolls hidden behind a natural philosophy shelf.
                  </p>
                  <p className="text-slate-400 text-xs md:text-sm">
                    In 1792, after tripping over Charlemagne's crown on the floor of his Paris property, Perseus commissioned <strong>Jonathan Whitmore the Third</strong> and <strong>Edmund Sterling</strong> to construct a 2,500 m² state-of-the-art vault in Switzerland. Today, this private vault is protected by UNESCO guidelines, Vatican treaties, and the Echelon Protocol.
                  </p>
                  <div className="pt-2 flex flex-wrap gap-3">
                    <button 
                      onClick={() => setActiveTab('vault')} 
                      className="px-4 py-2 bg-amber-500 hover:bg-amber-400 text-slate-950 font-semibold rounded-lg text-xs tracking-wide uppercase transition-all duration-150 flex items-center gap-2"
                    >
                      <Archive size={14} />
                      <span>Explore Swiss Vault & Visitors</span>
                      <ArrowRight size={14} />
                    </button>
                    <button 
                      onClick={() => setActiveTab('timeline')} 
                      className="px-4 py-2 bg-slate-800 hover:bg-slate-700 text-white rounded-lg text-xs font-semibold tracking-wide uppercase transition-all duration-150 flex items-center gap-2 border border-slate-700"
                    >
                      <Clock size={14} />
                      <span>Explore Historical Timeline</span>
                    </button>
                  </div>
                </div>
              </div>

              {/* Core Metrics grid */}
              <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
                
                <div className="p-5 bg-slate-900/50 rounded-xl border border-slate-800/80 space-y-3">
                  <div className="flex items-center justify-between text-slate-400">
                    <span className="text-xs font-mono uppercase">Swiss Vault Footprint</span>
                    <Building2 size={16} className="text-teal-400" />
                  </div>
                  <div className="space-y-1">
                    <h4 className="text-2xl font-black text-white">2,500 SQM</h4>
                    <p className="text-xs text-slate-400">Warehouse-sized facility commissioned in 1792 by Whitmore Sterling.</p>
                  </div>
                </div>

                <div className="p-5 bg-slate-900/50 rounded-xl border border-slate-800/80 space-y-3">
                  <div className="flex items-center justify-between text-slate-400">
                    <span className="text-xs font-mono uppercase">Vault Assets & Value</span>
                    <Coins size={16} className="text-amber-400" />
                  </div>
                  <div className="space-y-1">
                    <h4 className="text-2xl font-black text-white">Incalculable</h4>
                    <p className="text-xs text-slate-400">Includes Sun Tzu's scrolls, 47 Alexandria scrolls, Excalibur, and da Vinci's notebook.</p>
                  </div>
                </div>

                <div className="p-5 bg-slate-900/50 rounded-xl border border-slate-800/80 space-y-3">
                  <div className="flex items-center justify-between text-slate-400">
                    <span className="text-xs font-mono uppercase">Vatican & High Visitor Status</span>
                    <Coffee size={16} className="text-blue-400" />
                  </div>
                  <div className="space-y-1">
                    <h4 className="text-2xl font-black text-blue-400">PAPAL DIPLOMACY</h4>
                    <p className="text-xs text-slate-400">The Sovereign Pontiff regularly meets Perseus for private coffee visits.</p>
                  </div>
                </div>

              </div>

              {/* Whitmore Sterling 1792 Founding Narrative Section */}
              <div className="p-6 bg-slate-900/30 rounded-2xl border border-slate-800 space-y-4 shadow-xl">
                <div className="flex items-center justify-between border-b border-slate-800 pb-3">
                  <h3 className="text-base font-bold text-white flex items-center gap-2">
                    <Building2 size={18} className="text-amber-400" />
                    <span>The Founding of the Vault (Whitmore, Sterling & Associates — 1792)</span>
                  </h3>
                  <span className="text-xs font-mono bg-amber-950/80 text-amber-400 border border-amber-800/40 px-2.5 py-1 rounded">
                    London Historical Archives
                  </span>
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-6 text-xs text-slate-300 leading-relaxed">
                  <div className="space-y-3">
                    <p>
                      In October 1792, senior legal partner <strong>Jonathan Whitmore the Third</strong> received an extraordinary letter from Perseus Jackson complaining about disorganized properties in Paris, Edinburgh, Vienna, and London.
                    </p>
                    <blockquote className="border-l-2 border-amber-500 pl-3 italic text-slate-400 bg-slate-950/50 p-2.5 rounded">
                      "I put Charlemagne's crown on the floor of the Paris property in 1814 and forgot. It's currently on a table in Edinburgh next to a Mesopotamian dagger, Sun Tzu's bamboo slips, and three Shakespeare manuscripts... plus what might be a Holy Grail, but I'm genuinely not certain."
                      <span className="block text-[10px] font-mono text-amber-400 mt-1">— Perseus Jackson, Letter to Whitmore Sterling (1792)</span>
                    </blockquote>
                  </div>
                  <div className="space-y-3">
                    <p>
                      Junior partner <strong>Edmund Sterling</strong> cataloged the initial inventory: 17 bladed weapons, Sun Tzu's scrolls, 47 ancient Library of Alexandria papyri, Leonardo's notebook, Cleopatra's ring (carried in his coat pocket), 3 Parthenon fragments, a Babylon tile, and a crate from Jerusalem (33 AD).
                    </p>
                    <p className="text-slate-400">
                      Completed in 1803 by <strong>Jonathan Whitmore the Fourth</strong>, the 2,500 m² vault features dedicated climate pods for documents, weapons mounts, and a sealed room for the unopened Jerusalem crate.
                    </p>
                  </div>
                </div>
              </div>

            </div>
          )}

          {/* TAB 2: GLOBAL WORLD MAP */}
          {activeTab === 'map' && (
            <div className="space-y-6 animate-fadeIn">
              
              <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 bg-slate-900/40 p-4 rounded-xl border border-slate-800">
                <div className="space-y-1">
                  <h2 className="text-xl font-black text-white flex items-center gap-2">
                    <Globe className="text-teal-400" size={20} />
                    <span>Global Event Footprint Map</span>
                  </h2>
                  <p className="text-xs text-slate-400 font-mono">Interactive geolocation tracking of Perseus Jackson across 2,716 years of human history.</p>
                </div>

                {/* Map Era Filters */}
                <div className="flex items-center gap-1 bg-slate-950 p-1 rounded-lg border border-slate-800 text-xs">
                  <button 
                    onClick={() => setMapEraFilter('all')} 
                    className={`px-2.5 py-1 rounded transition-all ${mapEraFilter === 'all' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                  >
                    All Eras
                  </button>
                  <button 
                    onClick={() => setMapEraFilter('classical')} 
                    className={`px-2.5 py-1 rounded transition-all ${mapEraFilter === 'classical' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                  >
                    Classical
                  </button>
                  <button 
                    onClick={() => setMapEraFilter('medieval')} 
                    className={`px-2.5 py-1 rounded transition-all ${mapEraFilter === 'medieval' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                  >
                    Medieval
                  </button>
                  <button 
                    onClick={() => setMapEraFilter('early-modern')} 
                    className={`px-2.5 py-1 rounded transition-all ${mapEraFilter === 'early-modern' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                  >
                    Early Modern
                  </button>
                  <button 
                    onClick={() => setMapEraFilter('late-modern')} 
                    className={`px-2.5 py-1 rounded transition-all ${mapEraFilter === 'late-modern' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                  >
                    Late Modern
                  </button>
                </div>
              </div>

              {/* Interactive SVG World Map Canvas Container */}
              <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">
                
                {/* Visual Map Column */}
                <div className="lg:col-span-8 bg-slate-950 rounded-2xl border border-slate-800 p-4 relative min-h-[460px] flex flex-col justify-between overflow-hidden shadow-2xl">
                  
                  {/* Map Header Overlay */}
                  <div className="flex justify-between items-center z-10 text-[11px] font-mono text-slate-400 bg-slate-900/80 backdrop-blur px-3 py-1.5 rounded-lg border border-slate-800">
                    <span className="flex items-center gap-1.5 text-teal-400 font-bold">
                      <Crosshair size={12} className="animate-spin" /> ECHELON SATELLITE TACTICAL VECTOR MAP
                    </span>
                    <span>ACTIVE PINS: {filteredMapPins.length}</span>
                  </div>

                  {/* SVG World Map Graphics */}
                  <div className="relative w-full h-[380px] my-2 flex items-center justify-center bg-slate-950/90 rounded-xl overflow-hidden border border-slate-900">
                    
                    {/* Realistic World Map Path Representation */}
                    <svg viewBox="0 0 1000 500" className="w-full h-full object-cover">
                      {/* Gridlines */}
                      <defs>
                        <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
                          <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#1e293b" strokeWidth="0.5" />
                        </pattern>
                      </defs>
                      <rect width="1000" height="500" fill="url(#grid)" />
                      
                      {/* Equator & Meridian */}
                      <line x1="0" y1="250" x2="1000" y2="250" stroke="#334155" strokeWidth="1" strokeDasharray="4 4" />
                      <line x1="500" y1="0" x2="500" y2="500" stroke="#334155" strokeWidth="1" strokeDasharray="4 4" />

                      {/* Landmasses Pathing (Accurate Geographical Stylized Vectors) */}
                      <g className="fill-slate-800/80 stroke-teal-900/50 stroke-[1.2] transition-colors hover:fill-slate-800">
                        {/* North America */}
                        <path d="M 80,60 L 160,50 L 220,60 L 280,80 L 320,120 L 280,180 L 240,210 L 190,200 L 160,250 L 140,230 L 120,180 L 70,160 L 50,110 Z" />
                        {/* Greenland */}
                        <path d="M 330,30 L 390,25 L 410,60 L 360,90 L 320,60 Z" />
                        {/* South America */}
                        <path d="M 230,260 L 290,260 L 330,310 L 310,400 L 270,470 L 240,430 L 220,340 Z" />
                        {/* Europe */}
                        <path d="M 440,80 L 510,70 L 560,90 L 570,140 L 530,170 L 490,180 L 460,170 L 440,130 Z" />
                        {/* British Isles */}
                        <path d="M 450,115 L 470,110 L 465,140 L 445,135 Z" />
                        {/* Scandinavia */}
                        <path d="M 500,50 L 540,40 L 550,90 L 520,100 Z" />
                        {/* Africa */}
                        <path d="M 460,190 L 560,190 L 610,250 L 590,340 L 540,400 L 490,370 L 450,280 L 440,220 Z" />
                        {/* Asia / Eurasia */}
                        <path d="M 570,80 L 720,60 L 880,70 L 920,140 L 900,220 L 830,270 L 780,260 L 740,230 L 680,240 L 630,200 L 580,140 Z" />
                        {/* Indian Subcontinent */}
                        <path d="M 680,210 L 730,220 L 710,280 L 680,250 Z" />
                        {/* Japan */}
                        <path d="M 890,140 L 910,130 L 905,170 L 885,160 Z" />
                        {/* Australia & Oceania */}
                        <path d="M 780,330 L 870,320 L 890,390 L 830,430 L 770,380 Z" />
                      </g>
                    </svg>

                    {/* Interactive Pin Overlays mapped to SVG Coordinates */}
                    {filteredMapPins.map((pin) => {
                      const isSelected = selectedMapPin?.id === pin.id;
                      return (
                        <button
                          key={pin.id}
                          onClick={() => setSelectedMapPin(pin)}
                          style={{ left: `${pin.mapPos.x}%`, top: `${pin.mapPos.y}%` }}
                          className={`absolute transform -translate-x-1/2 -translate-y-1/2 group transition-all duration-200 z-20`}
                          title={`${pin.year}: ${pin.location}`}
                        >
                          <span className="relative flex h-5 w-5 items-center justify-center">
                            {isSelected && (
                              <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-teal-400 opacity-75"></span>
                            )}
                            <span className={`relative inline-flex rounded-full h-3.5 w-3.5 ${isSelected ? 'bg-amber-400 ring-4 ring-amber-500/40 scale-125' : 'bg-teal-400 group-hover:bg-teal-200 ring-2 ring-slate-950'}`}></span>
                          </span>
                          
                          {/* Label tooltip */}
                          <span className={`absolute top-6 left-1/2 transform -translate-x-1/2 text-[10px] font-mono font-bold px-2 py-0.5 rounded shadow-xl whitespace-nowrap transition-all border ${isSelected ? 'bg-amber-500 text-slate-950 border-amber-300 z-30 scale-105' : 'bg-slate-900/90 text-slate-300 border-slate-700 opacity-0 group-hover:opacity-100'}`}>
                            {pin.year} • {pin.id.toUpperCase()}
                          </span>
                        </button>
                      );
                    })}

                  </div>

                  {/* Map Footer status */}
                  <div className="z-10 flex justify-between items-center text-[10px] font-mono text-slate-500 border-t border-slate-900 pt-2">
                    <span>Click any tactical node to inspect reconstructed historical field logs.</span>
                    <span className="text-teal-400 font-bold">GRID CALIBRATED TO ~690 BCE ORIGIN</span>
                  </div>

                </div>

                {/* Selected Location Details Sidebar */}
                <div className="lg:col-span-4 bg-slate-900/40 p-6 rounded-2xl border border-slate-800 space-y-5 shadow-xl flex flex-col justify-between">
                  {selectedMapPin ? (
                    <div className="space-y-4 animate-fadeIn">
                      <div className="border-b border-slate-800 pb-3 space-y-1">
                        <div className="flex justify-between items-center">
                          <span className="text-xs bg-teal-950 text-teal-400 border border-teal-800/50 px-2 py-0.5 rounded font-mono font-bold">
                            {selectedMapPin.year}
                          </span>
                          <span className="text-[10px] font-mono bg-slate-800 text-slate-400 px-2 py-0.5 rounded capitalize">
                            {selectedMapPin.era} Era
                          </span>
                        </div>
                        <h3 className="text-lg font-bold text-white flex items-center gap-1.5 pt-1">
                          <MapPin size={16} className="text-amber-400 flex-shrink-0" />
                          <span>{selectedMapPin.location}</span>
                        </h3>
                      </div>

                      <div className="space-y-3 text-xs">
                        <div>
                          <span className="text-[10px] font-mono uppercase text-slate-500 block">Historical Figure Met</span>
                          <span className="text-white font-bold">{selectedMapPin.figure}</span>
                        </div>

                        {selectedMapPin.antic && (
                          <div>
                            <span className="text-[10px] font-mono uppercase text-amber-500 block flex items-center gap-1">
                              <Sparkles size={10} /> The Antic / Shenanigan
                            </span>
                            <p className="text-slate-300 leading-relaxed">{selectedMapPin.antic}</p>
                          </div>
                        )}

                        {selectedMapPin.advice && (
                          <div>
                            <span className="text-[10px] font-mono uppercase text-teal-400 block flex items-center gap-1">
                              <FileText size={10} /> Counsel & Tactical Advice
                            </span>
                            <p className="text-slate-300 italic">"{selectedMapPin.advice}"</p>
                          </div>
                        )}

                        <div className="bg-slate-950 p-3 rounded-lg border border-slate-800 space-y-1">
                          <span className="text-[10px] font-mono text-rose-400 font-bold uppercase block flex items-center gap-1">
                            <User size={10} /> Sassy Journal Note
                          </span>
                          <p className="text-slate-400 italic text-[11px] leading-normal">
                            "{selectedMapPin.sassyNote}"
                          </p>
                        </div>
                      </div>
                    </div>
                  ) : (
                    <div className="p-8 text-center text-slate-500 font-mono text-xs">
                      Select a pin on the map to inspect historical data.
                    </div>
                  )}

                  <div className="pt-2 border-t border-slate-800 flex items-center justify-between text-xs text-slate-400">
                    <span className="font-mono text-[10px]">Vault Artifact Link</span>
                    <span className="text-amber-400 font-bold text-xs">{selectedMapPin?.got}</span>
                  </div>
                </div>

              </div>

            </div>
          )}

          {/* TAB 3: INTERACTIVE TIMELINE */}
          {activeTab === 'timeline' && (
            <div className="space-y-6 animate-fadeIn">
              
              {/* Header and Filter Controls */}
              <div className="flex flex-col md:flex-row md:items-center justify-between gap-4 bg-slate-900/40 p-4 rounded-xl border border-slate-800">
                <div className="space-y-1">
                  <h2 className="text-xl font-black text-white flex items-center gap-2">
                    <Clock className="text-teal-400" size={20} />
                    <span>Historical Interventions & Antics</span>
                  </h2>
                  <p className="text-xs text-slate-400 font-mono">Filter and traverse 2,716 years of mapped historical footprints.</p>
                </div>

                {/* Filters */}
                <div className="flex flex-wrap items-center gap-3">
                  <div className="flex items-center gap-1 bg-slate-950 p-1 rounded-lg border border-slate-800 text-xs">
                    <button 
                      onClick={() => setSelectedEra('all')} 
                      className={`px-2.5 py-1 rounded transition-all ${selectedEra === 'all' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      All Eras
                    </button>
                    <button 
                      onClick={() => setSelectedEra('classical')} 
                      className={`px-2.5 py-1 rounded transition-all ${selectedEra === 'classical' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Classical
                    </button>
                    <button 
                      onClick={() => setSelectedEra('medieval')} 
                      className={`px-2.5 py-1 rounded transition-all ${selectedEra === 'medieval' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Medieval
                    </button>
                    <button 
                      onClick={() => setSelectedEra('early-modern')} 
                      className={`px-2.5 py-1 rounded transition-all ${selectedEra === 'early-modern' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Early Modern
                    </button>
                    <button 
                      onClick={() => setSelectedEra('late-modern')} 
                      className={`px-2.5 py-1 rounded transition-all ${selectedEra === 'late-modern' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Late Modern
                    </button>
                  </div>
                </div>
              </div>

              {/* Timeline Flow */}
              <div className="relative border-l-2 border-slate-800 ml-4 md:ml-12 pl-6 md:pl-8 space-y-12">
                {filteredTimeline.map((item, idx) => (
                  <div key={item.id} className="relative group">
                    
                    {/* Icon indicator in the line */}
                    <div className="absolute -left-[35px] md:-left-[43px] top-1.5 w-6 h-6 md:w-8 md:h-8 rounded-full bg-slate-950 border-2 border-teal-500 flex items-center justify-center text-teal-400 shadow-lg text-[10px] md:text-xs font-mono font-black group-hover:scale-110 transition-all">
                      {idx + 1}
                    </div>

                    {/* Card container */}
                    <div className="bg-slate-900/30 hover:bg-slate-900/60 transition-all duration-200 p-6 rounded-2xl border border-slate-800 hover:border-slate-700/80 shadow-xl space-y-4">
                      
                      {/* Top Header Row of card */}
                      <div className="flex flex-col md:flex-row md:items-center justify-between gap-2 border-b border-slate-800 pb-3">
                        <div className="flex items-center gap-2.5">
                          <span className="text-teal-400 font-black font-mono text-lg">{item.year}</span>
                          <span className="text-slate-500 font-mono text-xs">|</span>
                          <span className="text-slate-300 font-semibold text-sm flex items-center gap-1">
                            <MapPin size={12} className="text-slate-400" />
                            {item.location}
                          </span>
                        </div>
                        <div className="flex items-center gap-2">
                          <span className="text-xs bg-slate-800 text-slate-400 px-2 py-0.5 rounded font-mono capitalize">{item.era} Era</span>
                          <span className="text-xs bg-teal-950/80 text-teal-400 border border-teal-800/40 px-2 py-0.5 rounded font-mono">Reconstructed</span>
                        </div>
                      </div>

                      {/* Main Data grid inside card */}
                      <div className="grid grid-cols-1 lg:grid-cols-12 gap-6">
                        
                        {/* Details column */}
                        <div className="lg:col-span-8 space-y-4">
                          <div>
                            <h4 className="text-xs font-mono uppercase text-slate-500">Historical Figure Met</h4>
                            <p className="text-white font-bold text-sm">{item.figure}</p>
                          </div>

                          {item.antic && (
                            <div className="space-y-1">
                              <h4 className="text-xs font-mono uppercase text-amber-500 flex items-center gap-1">
                                <Sparkles size={12} />
                                <span>The Antic / Shenanigan</span>
                              </h4>
                              <p className="text-slate-300 text-sm leading-relaxed">{item.antic}</p>
                            </div>
                          )}

                          {item.advice && (
                            <div className="space-y-1">
                              <h4 className="text-xs font-mono uppercase text-teal-400 flex items-center gap-1">
                                <FileText size={12} />
                                <span>The Counsel / Advice Given</span>
                              </h4>
                              <p className="text-slate-300 text-sm italic">"{item.advice}"</p>
                            </div>
                          )}
                        </div>

                        {/* Artifact / Ledger column */}
                        <div className="lg:col-span-4 bg-slate-950/80 p-4 rounded-xl border border-slate-800 space-y-3 flex flex-col justify-between">
                          <div className="space-y-2">
                            <span className="text-[10px] text-slate-500 font-mono tracking-widest uppercase block border-b border-slate-800 pb-1.5">VAULT DIRECTORY SECURED</span>
                            <div>
                              <h4 className="text-xs text-amber-400 font-bold flex items-center gap-1">
                                <Archive size={12} />
                                <span>Artifact Acquired</span>
                              </h4>
                              <p className="text-white font-bold text-sm">{item.got}</p>
                            </div>
                          </div>

                          <div className="bg-slate-900/80 p-2.5 rounded border border-slate-800/80">
                            <h5 className="text-[10px] font-mono text-rose-400 font-bold uppercase tracking-wider mb-1 flex items-center gap-1">
                              <User size={10} />
                              <span>Sassy Journal Note</span>
                            </h5>
                            <p className="text-slate-400 text-xs italic leading-normal">"{item.sassyNote}"</p>
                          </div>
                        </div>

                      </div>

                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* TAB 4: SWISS VAULT DIRECTORY & VISITORS */}
          {activeTab === 'vault' && (
            <div className="space-y-8 animate-fadeIn">
              
              {/* Header and Search */}
              <div className="flex flex-col md:flex-row gap-4 items-center justify-between pb-2">
                <div>
                  <h1 className="text-2xl font-bold tracking-tight text-white flex items-center gap-2">
                    <Archive className="text-amber-500" />
                    The Swiss Vault & Visitors Directory
                  </h1>
                  <p className="text-sm text-slate-400">
                    Exploring the 2,500 m² private facility managed by Whitmore Sterling since 1792.
                  </p>
                </div>
                <div className="flex gap-2 w-full md:w-auto">
                  <input
                    type="text"
                    value={vaultSearch}
                    onChange={(e) => setVaultSearch(e.target.value)}
                    placeholder="Search inventory..."
                    className="w-full bg-slate-900 border border-slate-700 rounded-lg px-4 py-2 text-white placeholder-slate-500 focus:outline-none focus:ring-1 focus:ring-amber-500 text-xs font-mono"
                  />
                  <div className="flex items-center gap-1 bg-slate-900 p-1 rounded-lg border border-slate-800 text-xs">
                    <button 
                      onClick={() => setVaultCategory('all')} 
                      className={`px-2.5 py-1 rounded transition-all ${vaultCategory === 'all' ? 'bg-amber-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      All
                    </button>
                    <button 
                      onClick={() => setVaultCategory('weapons')} 
                      className={`px-2.5 py-1 rounded transition-all ${vaultCategory === 'weapons' ? 'bg-amber-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Weapons
                    </button>
                    <button 
                      onClick={() => setVaultCategory('documents')} 
                      className={`px-2.5 py-1 rounded transition-all ${vaultCategory === 'documents' ? 'bg-teal-500 text-slate-950 font-bold' : 'text-slate-400 hover:text-white'}`}
                    >
                      Docs
                    </button>
                  </div>
                </div>
              </div>

              {/* Documented Vault Visitors Ticker Table */}
              <div className="bg-slate-900/40 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-xl">
                <div className="flex justify-between items-center border-b border-slate-800 pb-3">
                  <h3 className="text-sm font-bold text-white flex items-center gap-2 font-mono">
                    <Users size={16} className="text-teal-400" />
                    <span>DOCUMENTED VAULT VISITORS (Public Banking Records — Past 50 Years)</span>
                  </h3>
                  <span className="text-[10px] font-mono text-slate-500">SEALED BY UNESCO & VATICAN</span>
                </div>

                <div className="overflow-x-auto">
                  <table className="w-full text-left text-xs font-mono">
                    <thead>
                      <tr className="border-b border-slate-800 text-slate-500 uppercase text-[10px]">
                        <th className="py-2 px-3">Date / Interval</th>
                        <th className="py-2 px-3">Visitor Entity</th>
                        <th className="py-2 px-3">Documented Purpose</th>
                        <th className="py-2 px-3 text-right">Clearance Level</th>
                      </tr>
                    </thead>
                    <tbody className="divide-y divide-slate-800/50 text-slate-300">
                      {VAULT_VISITORS.map((v, i) => (
                        <tr key={i} className="hover:bg-slate-800/30 transition-colors">
                          <td className="py-2.5 px-3 font-bold text-amber-400">{v.date}</td>
                          <td className="py-2.5 px-3 text-white font-semibold flex items-center gap-1.5">
                            {v.visitor.includes('Pontiff') && <Coffee size={12} className="text-amber-400" />}
                            {v.visitor}
                          </td>
                          <td className="py-2.5 px-3 text-slate-400 italic">{v.purpose}</td>
                          <td className="py-2.5 px-3 text-right text-teal-400 font-bold">{v.clearance}</td>
                        </tr>
                      ))}
                    </tbody>
                  </table>
                </div>
              </div>

              {/* Grid of Artifacts */}
              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {filteredVault.map((item) => (
                  <div key={item.id} className="p-5 bg-slate-900/40 rounded-xl border border-slate-800 hover:border-slate-700/60 transition-all flex flex-col justify-between space-y-4 shadow-lg group">
                    <div className="space-y-2">
                      <div className="flex justify-between items-start gap-2">
                        <span className="text-[10px] font-mono bg-slate-800 text-slate-400 px-2 py-0.5 rounded uppercase tracking-wider">{item.category}</span>
                        <span className="text-[10px] font-mono text-slate-500">ID: #{item.id}</span>
                      </div>
                      <h4 className="font-bold text-white text-base group-hover:text-teal-400 transition-colors">{item.name}</h4>
                      <p className="text-xs text-slate-400 font-mono flex items-center gap-1">
                        <MapPin size={10} />
                        {item.era}
                      </p>
                    </div>

                    <div className="space-y-3 pt-2 border-t border-slate-800/80">
                      <div className="bg-slate-950/60 p-3 rounded border border-slate-800">
                        <span className="text-[10px] font-mono text-rose-400 font-black uppercase tracking-wider block mb-1">Owner's Log Entry</span>
                        <p className="text-slate-300 text-xs italic leading-relaxed">"{item.note}"</p>
                      </div>

                      <div className="flex justify-between items-center text-xs font-mono">
                        <span className="text-slate-500 uppercase">Est. Value</span>
                        <span className="text-amber-400 font-bold">{item.val}</span>
                      </div>
                    </div>
                  </div>
                ))}
              </div>

            </div>
          )}

          {/* TAB 5: LEAKED FORUMS */}
          {activeTab === 'forum' && (
            <div className="space-y-8 animate-fadeIn">
              <div className="space-y-1">
                <h1 className="text-2xl font-bold text-white flex items-center gap-2">
                  <FileText className="text-teal-400" size={24} />
                  <span>Leaked Forums, Memos & Intel</span>
                </h1>
                <p className="text-sm text-slate-400">
                  Declassified communications and threads from r/TheEternalSoldier (4.7M members).
                </p>
              </div>

              <div className="grid grid-cols-1 lg:grid-cols-12 gap-8">
                
                {/* Reddit Thread Simulation */}
                <div className="lg:col-span-8 space-y-6">
                  
                  {/* Thread 1: Papal Coffee & Vault Visitors */}
                  <div className="bg-slate-900/30 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-xl">
                    <div className="flex justify-between items-center text-xs font-mono border-b border-slate-800 pb-3">
                      <div className="flex items-center gap-2">
                        <span className="text-teal-400 font-bold">r/TheEternalSoldier</span>
                        <span className="text-slate-500">•</span>
                        <span className="text-slate-400">Posted by u/SwissBankingAnon</span>
                      </div>
                      <span className="text-slate-500">3 Days Ago</span>
                    </div>

                    <div className="space-y-2">
                      <h3 className="text-lg font-bold text-white">Public Records Leak: The Pope had COFFEE with The Eternal Soldier in the Vault</h3>
                      <p className="text-xs text-slate-400 font-mono">Swiss Vault Access Log Reference: 1998 - 2018</p>
                    </div>

                    <p className="text-sm text-slate-300 leading-relaxed">
                      "I work near the Geneva banking district. The contents of the P. Jackson Trust vault are sealed beyond maximum clearance, but public records show visitor logs for the past 50 years. 
                      <span className="block my-2 p-2.5 bg-slate-950 rounded border border-slate-800 text-amber-400 font-mono text-xs">
                        Visitors: Heads of state, Directors of the Louvre & British Museum, UNESCO general counsel, and THE SOVEREIGN PONTIFF.
                      </span>
                      Apparently, they had coffee together. Given Perseus has been alive for over 2,700 years (since ~690 BCE) and has met every Pope since the Renaissance, they're literally old friends!"
                    </p>

                    <div className="flex items-center gap-4 text-xs font-mono text-slate-400 border-t border-slate-800 pt-3">
                      <span className="flex items-center gap-1 text-teal-400 font-bold"><ThumbsUp size={14} /> 5.8M upvotes</span>
                      <span>💬 342k comments</span>
                    </div>
                  </div>

                  {/* Thread 2: Summus Magister Maximus */}
                  <div className="bg-slate-900/30 p-6 rounded-2xl border border-slate-800 space-y-4 shadow-xl">
                    <div className="flex justify-between items-center text-xs font-mono border-b border-slate-800 pb-3">
                      <div className="flex items-center gap-2">
                        <span className="text-teal-400 font-bold">r/TheEternalSoldier</span>
                        <span className="text-slate-500">•</span>
                        <span className="text-slate-400">Posted by u/DeepStateDigger</span>
                      </div>
                      <span className="text-slate-500">2 Weeks Ago</span>
                    </div>

                    <div className="space-y-2">
                      <h3 className="text-lg font-bold text-white">We Found Petros's True Knights Templar Rank (And it's wild)</h3>
                      <p className="text-xs text-slate-400 font-mono">Vatican Archive scan reference: c. 1150 AD</p>
                    </div>

                    <p className="text-sm text-slate-300 leading-relaxed">
                      "Under all standard Templar ranks in the 1150 AD Vatican scroll, separated by a distinct line, is a custom ink entry: 
                      <span className="block my-2 p-2.5 bg-slate-950 rounded border border-slate-800 text-teal-400 font-mono text-xs">Summus Magister Maximus — Petros of Antioch</span>
                      He refused standard vows because he had 'taken more oaths than he could remember' and didn't want rank definitions because 'definitions become arguments.' Absolute legend."
                    </p>

                    <div className="flex items-center gap-4 text-xs font-mono text-slate-400 border-t border-slate-800 pt-3">
                      <span className="flex items-center gap-1 text-teal-400 font-bold"><ThumbsUp size={14} /> 4.1M upvotes</span>
                      <span>💬 127k comments</span>
                    </div>
                  </div>

                </div>

                {/* Official Memos Column */}
                <div className="lg:col-span-4 space-y-6">
                  
                  {/* Nigel Pierce Legal Memo */}
                  <div className="p-5 bg-slate-900/40 rounded-xl border border-slate-800 space-y-3">
                    <span className="text-[10px] text-amber-500 font-mono uppercase tracking-wider block border-b border-slate-800 pb-1.5 flex items-center gap-1">
                      <Shield size={10} /> LEGAL MEMO: WHITMORE STERLING (LONDON)
                    </span>
                    <h4 className="text-xs font-mono text-white">Senior Partner Nigel Pierce to US DNI Cartwright</h4>
                    <p className="text-xs text-slate-300 leading-relaxed italic">
                      "We have 450 years of institutional memory regarding Mr. Jackson. You Americans have 80 years and keep forgetting. Legal matters remain Whitmore Sterling's domain. Personal property remains Perseus's domain."
                    </p>
                  </div>

                  {/* French Fruit Basket Memo */}
                  <div className="p-5 bg-neutral-900/40 rounded-xl border border-neutral-800/80 space-y-3">
                    <span className="text-[10px] text-teal-400 font-mono uppercase tracking-wider block border-b border-neutral-800 pb-1.5 flex items-center gap-1">
                      <AlertCircle size={10} /> LEAKED INTELLIGENCE DRAFT
                    </span>
                    <div>
                      <h4 className="text-xs font-mono text-slate-400 uppercase">From: DGSE Director Claude</h4>
                      <h4 className="text-xs font-mono text-slate-400 uppercase">To: CIA Counterintelligence</h4>
                    </div>
                    <p className="text-xs text-slate-300 leading-relaxed italic">
                      "Enclosed please find a fresh fruit basket as a token of our sympathy for your recent Jackson incident. We know exactly how it feels. Best, Claude."
                    </p>
                  </div>

                </div>

              </div>
            </div>
          )}

          {/* TAB 6: TERMINAL */}
          {activeTab === 'terminal' && (
            <div className="space-y-6 animate-fadeIn">
              <div className="space-y-1">
                <h1 className="text-2xl font-bold text-white flex items-center gap-2 font-mono">
                  <Terminal className="text-teal-400" size={24} />
                  <span>Echelon Active Console</span>
                </h1>
                <p className="text-sm text-slate-400">
                  Project OMEGA tactical link. Enter the authorized Echelon code to simulate active protection.
                </p>
              </div>

              <div className="bg-slate-950 rounded-2xl border border-slate-800 overflow-hidden shadow-2xl flex flex-col h-[500px]">
                
                {/* Console header */}
                <div className="bg-slate-900 px-4 py-2 border-b border-slate-800 flex justify-between items-center text-xs font-mono text-slate-400">
                  <div className="flex items-center gap-2">
                    <span className="w-3 h-3 rounded-full bg-rose-500"></span>
                    <span className="w-3 h-3 rounded-full bg-amber-500"></span>
                    <span className="w-3 h-3 rounded-full bg-teal-500"></span>
                    <span className="ml-2">OMEGA_TACTICAL_TERMINAL.sh</span>
                  </div>
                  <span className="text-rose-400 font-bold">SECURE CHANNEL</span>
                </div>

                {/* Console logs */}
                <div className="flex-1 p-6 overflow-y-auto font-mono text-xs md:text-sm space-y-3 text-slate-300">
                  {terminalLogs.map((log, idx) => (
                    <div key={idx} className={`leading-relaxed whitespace-pre-wrap ${log.startsWith('>') ? 'text-teal-400 font-bold' : log.startsWith('⚠️') || log.includes('[CRITICAL ALERT]') ? 'text-rose-400 font-black' : log.startsWith('Morrison:') ? 'text-blue-400 font-semibold' : ''}`}>
                      {log}
                    </div>
                  ))}
                  <div ref={terminalEndRef} />
                </div>

                {/* Ghost Active Overlay */}
                {isGhostActive && (
                  <div className="bg-rose-950/20 border-t border-rose-800/50 p-4 text-center text-xs font-mono text-rose-400 uppercase tracking-widest animate-pulse">
                    🚨 ACTIVE EXTRACTION DETECTED: GHOST TEAMS IN EN ROUTE STATUS 🚨
                  </div>
                )}

                {/* Terminal input form */}
                <form onSubmit={handleTerminalSubmit} className="bg-slate-900/60 p-4 border-t border-slate-800 flex items-center gap-3">
                  <span className="text-teal-400 font-mono font-bold">{`omega_system@echelon:~$`}</span>
                  <input
                    type="text"
                    value={terminalInput}
                    onChange={(e) => setTerminalInput(e.target.value)}
                    placeholder="Enter Echelon passcode (november-hotel...)"
                    className="flex-1 bg-transparent border-none text-white focus:outline-none focus:ring-0 placeholder-slate-700 font-mono text-sm"
                  />
                  <button 
                    type="submit"
                    className="bg-teal-500 hover:bg-teal-400 text-slate-950 px-3 py-1.5 rounded-lg text-xs font-bold tracking-wider uppercase font-mono flex items-center gap-1.5 transition-colors"
                  >
                    <span>Execute</span>
                    <Send size={12} />
                  </button>
                </form>

              </div>

            </div>
          )}

        </main>
      </div>

      {/* Footer */}
      <footer className="border-t border-slate-800 bg-slate-950 px-6 py-4 flex flex-col md:flex-row justify-between items-center gap-4 text-xs font-mono text-slate-500">
        <div>
          <span>© 1678 - 2028 Whitmore, Sterling & Associates. All legal protections active.</span>
        </div>
        <div className="flex gap-4">
          <span>Security Class: OMEGA-CLEARANCE</span>
          <span>•</span>
          <span>We don't discuss Petros.</span>
        </div>
      </footer>

    </div>
  );
}