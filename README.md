<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web3 Ghostwriting Mastery | Command Center</title>
    <meta name="description" content="Harvard-style tracking dashboard for Web3 ghostwriting mastery.">
    
    <!-- React & Tailwind -->
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Icons -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lucide/0.263.1/lucide.min.js"></script>
    
    <!-- Google Fonts -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&family=JetBrains+Mono:wght@400;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0f172a; /* Slate 900 */
            color: #e2e8f0; /* Slate 200 */
        }
        
        .font-mono {
            font-family: 'JetBrains Mono', monospace;
        }

        .glass-panel {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(148, 163, 184, 0.1);
        }

        .neon-accent {
            box-shadow: 0 0 10px rgba(56, 189, 248, 0.3);
        }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a; 
        }
        ::-webkit-scrollbar-thumb {
            background: #334155; 
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #475569; 
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;
        const { BookOpen, Shield, Zap, BarChart3, CheckCircle2, PenTool, Terminal, Users, Brain, Lock } = lucide;

        // --- DATA & CONTENT ---

        const LEXICON = {
            power: [
                "Undeniable", "Essential", "Proven", "Framework", "Blueprint", 
                "Alpha", "Utility", "Consensus", "On-chain", "Permissionless",
                "Dangerous", "Uncomfortable", "Counter-intuitive", "However", "Consequently"
            ],
            weak: [
                "I think", "Maybe", "Sort of", "Hopefully", "Just wondering", 
                "Moon", "Guaranteed", "Next 100x", "Fast money", "Lambos",
                "Like this if", "Share this", "Tag a friend"
            ]
        };

        const CURRICULUM = [
            {
                week: 1,
                title: "Foundations & Algo Intelligence",
                days: [
                    { day: "Mon", title: "Audit & Avatar", task: "Analyze 3 Top Voices (Ghostwriter) / Explain 3 Narratives (Web3 Boy). Create Voice Profile.", type: "Joint" },
                    { day: "Tue", title: "FB Algo Deep Dive", task: "Optimize Bio with Trust Signal. Pin 'Hero's Journey' post. Archive dead posts.", type: "Joint" },
                    { day: "Wed", title: "The Hook Lab", task: "Write 20 hooks for 1 piece of content. Test: Vulnerability + Benefit.", type: "Writing" },
                    { day: "Thu", title: "Translation Drill", task: "Create Translation Dictionary. Rewrite complex tech update into viral post < 15 mins.", type: "Joint" },
                    { day: "Fri", title: "Engagement Asset", task: "Post 'Contrarian Take'. Comment on 5 big accounts before posting.", type: "Action" },
                    { day: "Sat", title: "Weekly Review", task: "Analyze metrics. Which phrase won? Lock Week 2 topics.", type: "Review" },
                    { day: "Sun", title: "Neural Reset", task: "NO CONSUMPTION. Production or Rest only.", type: "Rest" },
                ]
            },
            {
                week: 2,
                title: "Content Engineering",
                days: [
                    { day: "Mon", title: "The Interview", task: "Ghostwriter interviews Web3 Boy (30m). Transcribe -> 1 Article, 3 Posts, 5 Tweets.", type: "Joint" },
                    { day: "Tue", title: "Visual Psychology", task: "Create 3 Carousel PDFs (Tokenomic Flow or Security Setup).", type: "Design" },
                    { day: "Wed", title: "Word Workshop", task: "Rewrite old content using Lexicon of Power. Inject Sensory Words.", type: "Writing" },
                    { day: "Thu", title: "Community Raid", task: "Post high-value comments in 5 Web3 Groups. Aim for 'Top Contributor'.", type: "Action" },
                    { day: "Fri", title: "Social Proof", task: "Post client win or trading result. 'Results speak louder than roadmaps.'", type: "Action" },
                    { day: "Sat", title: "Crisis Sim", task: "Draft response to a fake crisis/FUD using Harvard Negotiation tactics.", type: "Simulation" },
                    { day: "Sun", title: "Neural Reset", task: "NO CONSUMPTION. Production or Rest only.", type: "Rest" },
                ]
            },
            {
                week: 3,
                title: "Visibility & Status",
                days: [
                    { day: "Mon", title: "Cross-Pollination", task: "Screenshot best Tweet -> Post to FB (Link in comments).", type: "Action" },
                    { day: "Tue", title: "The Manifesto", task: "Write 1,500+ word definitive guide (State of DeFi / Future of Writing).", type: "Writing" },
                    { day: "Wed", title: "Whale Hunting", task: "Send 10 Value-First DMs to founders. (Rewritten hooks, free value).", type: "Outreach" },
                    { day: "Thu", title: "Algo Stress Test", task: "Post 3x (Morning, Lunch, Night). Track velocity.", type: "Data" },
                    { day: "Fri", title: "Evergreen Loop", task: "Build Content Bank. Schedule reposts of Week 1 hits.", type: "System" },
                    { day: "Sat", title: "Graduation", task: "Final Review. Did we hit goals? Set 3-month plan.", type: "Review" },
                    { day: "Sun", title: "Neural Reset", task: "Celebrate.", type: "Rest" },
                ]
            }
        ];

        // --- COMPONENTS ---

        const ProgressBar = ({ progress }) => (
            <div className="w-full bg-slate-800 h-2 rounded-full overflow-hidden mt-4">
                <div 
                    className="bg-cyan-500 h-full transition-all duration-500 ease-out neon-accent"
                    style={{ width: `${progress}%` }}
                ></div>
            </div>
        );

        const TabButton = ({ active, label, onClick, icon: Icon }) => (
            <button 
                onClick={onClick}
                className={`flex items-center gap-2 px-6 py-3 rounded-lg font-medium transition-all duration-200 ${
                    active 
                    ? 'bg-cyan-500/10 text-cyan-400 border border-cyan-500/50 neon-accent' 
                    : 'text-slate-400 hover:text-slate-200 hover:bg-slate-800'
                }`}
            >
                <Icon size={18} />
                {label}
            </button>
        );

        const CopyAuditor = () => {
            const [text, setText] = useState("");
            const [analysis, setAnalysis] = useState(null);

            const analyzeText = () => {
                let highlightedHtml = text;
                let weakCount = 0;
                let powerCount = 0;

                LEXICON.weak.forEach(word => {
                    const regex = new RegExp(`\\b${word}\\b`, 'gi');
                    if (text.match(regex)) {
                        weakCount += (text.match(regex) || []).length;
                        highlightedHtml = highlightedHtml.replace(regex, `<span class="bg-red-500/20 text-red-300 px-1 rounded border-b border-red-500 cursor-help" title="Avoid this!">${word}</span>`);
                    }
                });

                LEXICON.power.forEach(word => {
                    const regex = new RegExp(`\\b${word}\\b`, 'gi');
                    if (text.match(regex)) {
                        powerCount += (text.match(regex) || []).length;
                        highlightedHtml = highlightedHtml.replace(regex, `<span class="bg-green-500/20 text-green-300 px-1 rounded border-b border-green-500">${word}</span>`);
                    }
                });

                setAnalysis({ html: highlightedHtml, weakCount, powerCount });
            };

            return (
                <div className="space-y-6 animate-in fade-in duration-500">
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div className="glass-panel p-6 rounded-xl">
                            <h3 className="text-lg font-semibold text-cyan-400 mb-4 flex items-center gap-2">
                                <PenTool size={20} /> Content Input
                            </h3>
                            <textarea
                                value={text}
                                onChange={(e) => setText(e.target.value)}
                                placeholder="Paste your draft here to audit against the Harvard Lexicon..."
                                className="w-full h-64 bg-slate-900/50 border border-slate-700 rounded-lg p-4 text-slate-300 focus:outline-none focus:border-cyan-500 font-mono text-sm resize-none"
                            ></textarea>
                            <button 
                                onClick={analyzeText}
                                className="mt-4 w-full bg-cyan-600 hover:bg-cyan-500 text-white font-bold py-2 px-4 rounded-lg transition-colors flex items-center justify-center gap-2"
                            >
                                <Zap size={18} /> Run Audit
                            </button>
                        </div>

                        <div className="glass-panel p-6 rounded-xl">
                            <h3 className="text-lg font-semibold text-purple-400 mb-4 flex items-center gap-2">
                                <Brain size={20} /> Analysis Results
                            </h3>
                            {analysis ? (
                                <div>
                                    <div className="flex gap-4 mb-4">
                                        <div className="flex-1 bg-red-900/20 border border-red-900/50 p-3 rounded text-center">
                                            <div className="text-2xl font-bold text-red-400">{analysis.weakCount}</div>
                                            <div className="text-xs text-red-300/70 uppercase tracking-wider">Weak Signals</div>
                                        </div>
                                        <div className="flex-1 bg-green-900/20 border border-green-900/50 p-3 rounded text-center">
                                            <div className="text-2xl font-bold text-green-400">{analysis.powerCount}</div>
                                            <div className="text-xs text-green-300/70 uppercase tracking-wider">Power Signals</div>
                                        </div>
                                    </div>
                                    <div 
                                        className="prose prose-invert max-w-none text-sm font-mono p-4 bg-slate-950 rounded-lg border border-slate-800 min-h-[160px]"
                                        dangerouslySetInnerHTML={{ __html: analysis.html || "Waiting for input..." }}
                                    />
                                </div>
                            ) : (
                                <div className="h-full flex flex-col items-center justify-center text-slate-500 space-y-2">
                                    <Shield size={48} className="opacity-20" />
                                    <p>Ready to audit your copy.</p>
                                </div>
                            )}
                        </div>
                    </div>
                </div>
            );
        };

        const AlgoCheatSheet = () => (
            <div className="grid grid-cols-1 md:grid-cols-3 gap-6 animate-in fade-in duration-500">
                <div className="glass-panel p-6 rounded-xl border-l-4 border-cyan-500">
                    <h3 className="font-bold text-lg mb-2 text-white">The Ranking Hierarchy</h3>
                    <ul className="space-y-2 text-slate-300 text-sm">
                        <li className="flex items-center gap-2"><span className="text-cyan-400 font-mono">01.</span> Comments (5+ words)</li>
                        <li className="flex items-center gap-2"><span className="text-cyan-400 font-mono">02.</span> Shares (with caption)</li>
                        <li className="flex items-center gap-2"><span className="text-cyan-400 font-mono">03.</span> Watch Time (Video)</li>
                        <li className="flex items-center gap-2 text-slate-500"><span className="font-mono">04.</span> Passive Likes (Low value)</li>
                    </ul>
                </div>
                <div className="glass-panel p-6 rounded-xl border-l-4 border-red-500">
                    <h3 className="font-bold text-lg mb-2 text-white">Trust Killers</h3>
                    <ul className="space-y-2 text-slate-300 text-sm">
                        <li className="flex items-center gap-2"><span className="text-red-400">×</span> "Link in bio" in caption</li>
                        <li className="flex items-center gap-2"><span className="text-red-400">×</span> Engagement Bait ("Tag a friend")</li>
                        <li className="flex items-center gap-2"><span className="text-red-400">×</span> External Links (Kill reach)</li>
                        <li className="flex items-center gap-2"><span className="text-red-400">×</span> Editing post in first 10m</li>
                    </ul>
                </div>
                <div className="glass-panel p-6 rounded-xl border-l-4 border-green-500">
                    <h3 className="font-bold text-lg mb-2 text-white">2025 Hacks</h3>
                    <ul className="space-y-2 text-slate-300 text-sm">
                        <li className="flex items-center gap-2"><span className="text-green-400">✓</span> Reply to comments instantly</li>
                        <li className="flex items-center gap-2"><span className="text-green-400">✓</span> Use "Text on Background" format</li>
                        <li className="flex items-center gap-2"><span className="text-green-400">✓</span> Warm up account 15m before posting</li>
                        <li className="flex items-center gap-2"><span className="text-green-400">✓</span> Carousel PDFs for retention</li>
                    </ul>
                </div>
            </div>
        );

        const CurriculumTracker = ({ tasks, completed, toggleTask }) => {
            return (
                <div className="space-y-8 animate-in fade-in duration-500">
                    {CURRICULUM.map((week) => (
                        <div key={week.week} className="glass-panel rounded-xl overflow-hidden">
                            <div className="bg-slate-800/50 p-4 border-b border-slate-700 flex justify-between items-center">
                                <h3 className="font-bold text-lg text-white flex items-center gap-2">
                                    <span className="bg-cyan-500 text-black text-xs font-bold px-2 py-1 rounded">WEEK {week.week}</span>
                                    {week.title}
                                </h3>
                                <div className="text-xs font-mono text-slate-400 uppercase tracking-widest">
                                    Harvard Stream
                                </div>
                            </div>
                            <div className="divide-y divide-slate-800/50">
                                {week.days.map((day, idx) => {
                                    const id = `w${week.week}-d${idx}`;
                                    const isDone = completed[id];
                                    return (
                                        <div key={id} className={`p-4 flex items-start gap-4 group hover:bg-slate-800/30 transition-colors ${isDone ? 'opacity-50 grayscale' : ''}`}>
                                            <button 
                                                onClick={() => toggleTask(id)}
                                                className={`mt-1 flex-shrink-0 w-6 h-6 rounded-full border-2 flex items-center justify-center transition-all ${
                                                    isDone 
                                                    ? 'bg-cyan-500 border-cyan-500 text-black' 
                                                    : 'border-slate-600 text-transparent hover:border-cyan-400'
                                                }`}
                                            >
                                                <CheckCircle2 size={16} />
                                            </button>
                                            <div className="flex-1">
                                                <div className="flex items-center gap-2 mb-1">
                                                    <span className="font-mono text-xs text-cyan-400 font-bold">{day.day}</span>
                                                    <span className="text-xs bg-slate-800 text-slate-400 px-2 py-0.5 rounded border border-slate-700">{day.type}</span>
                                                </div>
                                                <h4 className={`font-semibold ${isDone ? 'line-through text-slate-500' : 'text-slate-200'}`}>{day.title}</h4>
                                                <p className="text-sm text-slate-400 mt-1">{day.task}</p>
                                            </div>
                                        </div>
                                    );
                                })}
                            </div>
                        </div>
                    ))}
                </div>
            );
        };

        const App = () => {
            const [activeTab, setActiveTab] = useState("tracker");
            const [completedTasks, setCompletedTasks] = useState({});

            // Load from local storage on mount
            useEffect(() => {
                const saved = localStorage.getItem('web3_mastery_progress');
                if (saved) setCompletedTasks(JSON.parse(saved));
            }, []);

            // Save to local storage
            const toggleTask = (id) => {
                const newCompleted = { ...completedTasks, [id]: !completedTasks[id] };
                setCompletedTasks(newCompleted);
                localStorage.setItem('web3_mastery_progress', JSON.stringify(newCompleted));
            };

            const totalTasks = CURRICULUM.reduce((acc, week) => acc + week.days.length, 0);
            const doneCount = Object.values(completedTasks).filter(Boolean).length;
            const progress = Math.round((doneCount / totalTasks) * 100);

            return (
                <div className="min-h-screen pb-12">
                    {/* Header */}
                    <header className="border-b border-slate-800 bg-slate-950/50 backdrop-blur-sm sticky top-0 z-10">
                        <div className="max-w-5xl mx-auto px-6 py-4">
                            <div className="flex justify-between items-center mb-4">
                                <div>
                                    <h1 className="text-2xl font-bold text-white tracking-tight flex items-center gap-2">
                                        <Terminal className="text-cyan-500" />
                                 
