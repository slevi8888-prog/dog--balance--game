import React, { useMemo, useState } from "react";
import { createRoot } from "react-dom/client";
import { motion } from "framer-motion";
import { Bone, Heart, Dumbbell, Utensils, Home, Footprints, RotateCcw, Bell } from "lucide-react";
import "./style.css";

const clamp = (value, min = 0, max = 100) => Math.max(min, Math.min(max, value));

const breedProfiles = {
  "בורדר קולי": { energy: 85, calm: 35, bond: 70, tired: 20, note: "גזע אנרגטי וחכם מאוד שצריך פריקה ומנטליות." },
  "מלינואה": { energy: 92, calm: 28, bond: 65, tired: 18, note: "כלב עבודה עם דרייב גבוה מאוד." },
  "פודל": { energy: 68, calm: 48, bond: 76, tired: 22, note: "כלב חכם מאוד, רגיש, חברתי וזקוק לגירוי מנטלי וקשר." },
  "לברדור": { energy: 72, calm: 52, bond: 78, tired: 25, note: "חברותי מאוד וזקוק לפעילות וקשר." },
  "שיצו": { energy: 45, calm: 65, bond: 75, tired: 30, note: "כלב לוויה רגוע יחסית." },
  "מעורב": { energy: 60, calm: 50, bond: 60, tired: 25, note: "כל כלב מעורב שונה בהתאם לאופי ולסביבה." },
};

const missions = [
  "המטרה היום: לבנות לכלב יום מאוזן — פריקה, תקשורת ומנוחה.",
  "המטרה היום: לא רק לעייף את הכלב, אלא גם ללמד אותו להירגע.",
  "המטרה היום: לשלב פעילות, אוכל, קשר ומנוחה בלי להציף את הכלב.",
];

const professionalTerms = [
  { title: "עוררות", description: "המצב הרגשי והאנרגטי של הכלב. עוררות גבוהה מדי יכולה לגרום לקושי להירגע ולהקשיב." },
  { title: "סף תגובתיות", description: "הנקודה שבה הכלב כבר מגיב לגירוי וקשה יותר להחזיר אותו לרוגע." },
  { title: "גירוי", description: "כל דבר שהכלב מגיב אליו — אנשים, כלבים, רעשים, כדור, אוכל ועוד." },
  { title: "דרייב", description: "המוטיבציה הפנימית של הכלב לפעול — משחק, מזון, ציד, עבודה או תנועה." },
  { title: "חיזוק", description: "משהו שהכלב אוהב ולכן מגדיל סיכוי שהתנהגות תחזור שוב." },
];

function Button({ children, className = "", ...props }) {
  return <button className={`btn ${className}`} {...props}>{children}</button>;
}

function Card({ children, className = "" }) {
  return <div className={`card ${className}`}>{children}</div>;
}

function StatBar({ label, value }) {
  return (
    <div className="stat">
      <div className="stat-top"><span>{label}</span><span>{value}/100</span></div>
      <div className="bar"><motion.div className="bar-fill" initial={{ width: 0 }} animate={{ width: `${value}%` }} transition={{ duration: 0.35 }} /></div>
    </div>
  );
}

function detectBreed(text) {
  const input = (text || "").trim();
  const found = Object.keys(breedProfiles).find((breed) => input.includes(breed));
  return found || "מעורב";
}

export default function DogBalanceGame() {
  const playSound = (type) => {
    try {
      const audio = new Audio(
        type === "success"
          ? "https://assets.mixkit.co/active_storage/sfx/2000/2000-preview.mp3"
          : "https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3"
      );
      audio.volume = 0.22;
      audio.play();
    } catch {}
  };

  const [selectedDog, setSelectedDog] = useState(null);
  const [dogName, setDogName] = useState("");
  const [breed, setBreed] = useState("");
  const [dogAge, setDogAge] = useState("");
  const [day, setDay] = useState(1);
  const [turns, setTurns] = useState(0);
  const [history, setHistory] = useState([]);
  const [scene, setScene] = useState({ emoji: "🐶", title: "הכלב מחכה להחלטה שלך", text: "בחר פעולה ותראה מה קורה לו במהלך היום." });
  const [notification, setNotification] = useState(null);
  const [tasks, setTasks] = useState({
    morningWalk: false,
    morningFood: false,
    mentalWork: false,
    eveningFood: false,
    eveningWalk: false,
    rest: false,
  });
  const [stats, setStats] = useState({ energy: 60, calm: 40, bond: 50, tired: 20, hunger: 35 });

  const mission = missions[(day - 1) % missions.length];

  const balanceScore = useMemo(() => {
    const energyBalance = 100 - Math.abs(stats.energy - 45);
    const calmWeight = stats.calm * 1.2;
    const bondWeight = stats.bond;
    const tiredPenalty = stats.tired > 80 ? (stats.tired - 80) * 1.5 : 0;
    const hungerPenalty = stats.hunger > 75 ? (stats.hunger - 75) * 1.5 : 0;
    return clamp(Math.round((energyBalance + calmWeight + bondWeight) / 3 - tiredPenalty - hungerPenalty));
  }, [stats]);

  const dogMood = useMemo(() => {
    if (stats.hunger > 80) return "הכלב רעב וקשה לו להתרכז.";
    if (stats.energy > 80 && stats.calm < 35) return "הכלב מוצף באנרגיה וקשה להשתלט עליו.";
    if (stats.tired > 85) return "הכלב עייף מדי, עכשיו הוא צריך מנוחה ולא עוד דרישות.";
    if (stats.calm > 70 && stats.bond > 65) return "הכלב רגוע, קשוב ומחובר אליך.";
    if (stats.bond < 35) return "הקשר נמוך — הכלב פחות מחפש שיתוף פעולה.";
    return "הכלב במצב סביר, אבל היום עוד לא מאוזן לגמרי.";
  }, [stats]);

  const triggerNotification = (type) => {
    const name = selectedDog?.name || "הכלב שלך";
    const messages = {
      no_morning_walk: { emoji: "🌅🦮", title: "טיול בוקר עדיין לא בוצע", text: `${name} עוד לא יצא לטיול בוקר. זה הזמן להתחיל לפרוק אנרגיה בצורה נכונה.` },
      no_evening_walk: { emoji: "🌙🦮", title: "טיול ערב חסר", text: `${name} צריך סגירת יום רגועה עם טיול ערב קצר או ארוך לפי האנרגיה שלו.` },
      no_food: { emoji: "🍖", title: "ארוחה חסרה", text: `${name} עדיין לא קיבל את אחת הארוחות שלו היום.` },
      high_energy: { emoji: "⚡", title: "הכלב באנרגיה גבוהה", text: `${name} צריך פריקת אנרגיה או פעילות משותפת.` },
      bark: { emoji: "🐶🔊", title: "הכלב נובח בבית", text: `${name} מתחיל לנבוח ולחפש פריקה או רוגע.` },
      balanced: { emoji: "😌", title: "יום מאוזן", text: `כל הכבוד. היום עזרת ל-${name} להיות רגוע ומאוזן יותר.` },
    };
    setNotification(messages[type]);
    if (type === "bark") {
      try {
        const barkAudio = new Audio("https://assets.mixkit.co/active_storage/sfx/2354/2354-preview.mp3");
        barkAudio.volume = 0.35;
        barkAudio.play();
      } catch {}
    }
  };

  const startGame = () => {
    const detectedBreed = detectBreed(breed);
    const breedData = breedProfiles[detectedBreed] || breedProfiles["מעורב"];
    const ageNumber = Number(dogAge);
    const isPuppy = ageNumber <= 1;
    const isSenior = ageNumber >= 8;

    let energy = breedData.energy;
    let calm = breedData.calm;
    let tired = breedData.tired;

    if (isPuppy) { energy += 12; calm -= 10; }
    if (isSenior) { energy -= 15; tired += 15; calm += 8; }

    setSelectedDog({ name: dogName || "הכלב שלך", breed: detectedBreed, note: breedData.note, emoji: "🐶" });
    setStats({ energy: clamp(energy), calm: clamp(calm), bond: breedData.bond, tired: clamp(tired), hunger: 40 });
    setDay(1);
    setTurns(0);
    setTasks({ morningWalk: false, morningFood: false, mentalWork: false, eveningFood: false, eveningWalk: false, rest: false });
    setHistory([`${dogName || "הכלב שלך"} הוא ${breed || "מעורב"} — זוהה כ${detectedBreed}. ${breedData.note}`]);
    setScene({ emoji: "🐶", title: "היום מתחיל", text: "עכשיו מתחילים לבנות לכלב סדר יום נכון ומאוזן." });
  };

  const addHistory = (text) => setHistory((prev) => [text, ...prev].slice(0, 6));
  const markTask = (key) => setTasks((prev) => ({ ...prev, [key]: true }));

  const applyAction = (action) => {
    let next = { ...stats };
    let message = "";

    if (action === "morning_walk") {
      markTask("morningWalk");
      setScene({ emoji: "🌅🧍‍♂️🦮", title: "טיול בוקר", text: "פתחתם את היום עם תנועה, ריח ופריקה ראשונית." });
      next.energy = clamp(next.energy - 14); next.calm = clamp(next.calm + 8); next.bond = clamp(next.bond + 5); next.tired = clamp(next.tired + 8); next.hunger = clamp(next.hunger + 5);
      message = "טיול בוקר עזר לפתוח את היום בצורה מאוזנת ולהוריד עומס אנרגטי.";
    }

    if (action === "evening_walk") {
      markTask("eveningWalk");
      setScene({ emoji: "🌙🧍‍♂️🦮", title: "טיול ערב", text: "טיול ערב עוזר לסגור את היום ולהכניס את הכלב לשגרה רגועה יותר." });
      next.energy = clamp(next.energy - 18); next.calm = clamp(next.calm + 12); next.bond = clamp(next.bond + 6); next.tired = clamp(next.tired + 10); next.hunger = clamp(next.hunger + 5);
      message = "טיול ערב עזר לפרוק שאריות אנרגיה ולהכין את הכלב למנוחה.";
    }

    if (action === "long_walk") {
      markTask("morningWalk");
      setScene({ emoji: "🧍‍♂️🦮🌳🏃", title: "טיול ארוך / פריקה", text: "הכלב יצא לפריקה אמיתית, הוציא אנרגיה וחזר פנוי יותר לרוגע." });
      next.energy = clamp(next.energy - 24); next.calm = clamp(next.calm + 14); next.bond = clamp(next.bond + 8); next.tired = clamp(next.tired + 18); next.hunger = clamp(next.hunger + 10);
      message = "טיול ארוך ופריקת אנרגיה נכונה עזרו לכלב להתאזן ולהיות רגוע יותר.";
    }

    if (action === "shared_play") {
      setScene({ emoji: "🧍‍♂️🐕🎾", title: "משחק משותף", text: "אתם משחקים יחד. הקשר מתחזק, אבל חשוב לעצור לפני שהכלב עולה מדי." });
      next.energy = clamp(next.energy - 12); next.bond = clamp(next.bond + 12); next.tired = clamp(next.tired + 10); next.calm = clamp(next.calm - (stats.calm < 35 ? 8 : 3)); next.hunger = clamp(next.hunger + 5);
      message = stats.calm < 35 ? "משחק כשהכלב כבר לא רגוע עלול להרים עוררות. צריך לדעת לעצור בזמן." : "משחק משותף חיזק קשר ופרק קצת אנרגיה.";
    }

    if (action === "morning_food" || action === "evening_food") {
      markTask(action === "morning_food" ? "morningFood" : "eveningFood");
      setScene({ emoji: "🐶🥣", title: action === "morning_food" ? "מזון בוקר" : "מזון ערב", text: "הכלב אוכל מהקערה. תזמון נכון של אוכל משפיע על רוגע ופניות." });
      if (stats.hunger < 25) {
        next.calm = clamp(next.calm - 8); next.energy = clamp(next.energy + 10);
        message = "נתת יותר מדי אוכל בזמן קצר. האכלת יתר יכולה ליצור אי נוחות וחוסר איזון.";
      } else if (stats.energy > 70) {
        next.hunger = clamp(next.hunger - 30); next.calm = clamp(next.calm + 3); next.energy = clamp(next.energy + 6);
        message = "לתת אוכל לפני פריקת אנרגיה מלאה זה לא אידיאלי. פעילות אינטנסיבית אחרי אוכל עלולה להיות מסוכנת אצל חלק מהכלבים.";
      } else {
        next.hunger = clamp(next.hunger - 35); next.calm = clamp(next.calm + 5); next.energy = clamp(next.energy + 6);
        message = "אוכל בזמן נכון עוזר לכלב להיות רגוע ופנוי יותר ללמידה.";
      }
    }

    if (action === "mental_work") {
      markTask("mentalWork");
      setScene({ emoji: "🐶🧠🧩", title: "פריקה מנטלית", text: "הכלב עובד עם הראש, חושב, מתרכז ומתעייף בצורה איכותית." });
      const success = stats.energy < 80 && stats.tired < 75 && stats.hunger < 75;
      next.bond = clamp(next.bond + (success ? 14 : 5)); next.calm = clamp(next.calm + (success ? 8 : -6)); next.tired = clamp(next.tired + 10); next.energy = clamp(next.energy - 8); next.hunger = clamp(next.hunger + 5);
      message = success ? "פריקת אנרגיה מנטלית בזמן נכון עזרה לכלב להתרכז, לחשוב ולהתאזן." : "הפריקה המנטלית פחות הצליחה כי הכלב לא היה פנוי — רעב, עייף או מוצף מדי.";
    }

    if (action === "place") {
      markTask("rest");
      setScene({ emoji: "🐶🛏️😌", title: "מקום / מנוחה", text: "הכלב נשלח למקום, נרגע ולומד שגם מנוחה היא חלק מהיום." });
      next.calm = clamp(next.calm + 18); next.energy = clamp(next.energy + 4); next.tired = clamp(next.tired - 12); next.bond = clamp(next.bond + 4); next.hunger = clamp(next.hunger + 4);
      message = "שליחה למקום ומנוחה אחרי פעילות בונה כלב רגוע יותר, לא רק עייף יותר.";
    }

    const improved = next.calm > stats.calm || next.bond > stats.bond;
    playSound(improved ? "success" : "warning");

    if (!tasks.morningWalk && turns >= 2) triggerNotification("no_morning_walk");
    else if ((tasks.morningFood === false || tasks.eveningFood === false) && turns >= 4) triggerNotification("no_food");
    else if (!tasks.eveningWalk && turns >= 5) triggerNotification("no_evening_walk");
    else if (next.energy > 85 && next.calm < 35) triggerNotification("bark");
    else if (next.energy > 80) triggerNotification("high_energy");
    else if (next.calm > 75 && next.bond > 70) triggerNotification("balanced");

    setStats(next);
    setTurns((t) => t + 1);
    addHistory(message);
  };

  const nextDay = () => {
    setDay((d) => d + 1);
    setTurns(0);
    setTasks({ morningWalk: false, morningFood: false, mentalWork: false, eveningFood: false, eveningWalk: false, rest: false });
    setStats((s) => ({ energy: clamp(s.energy + 25), calm: clamp(s.calm - 12), bond: clamp(s.bond - 5), tired: clamp(s.tired - 35), hunger: clamp(s.hunger + 35) }));
    setHistory((prev) => [`יום חדש התחיל. כלב צריך שוב סדר, פריקה, תקשורת ומנוחה.`, ...prev].slice(0, 6));
    setNotification(null);
  };

  const reset = () => {
    setSelectedDog(null); setDay(1); setTurns(0); setHistory([]); setNotification(null);
  };

  if (!selectedDog) {
    return (
      <div dir="rtl" className="page center">
        <Card className="hero-card">
          <div className="hero">
            <motion.div initial={{ scale: 0.9, opacity: 0 }} animate={{ scale: 1, opacity: 1 }} className="hero-emoji">🐶🎮</motion.div>
            <h1>מה הכלב שלך צריך היום?</h1>
            <p>משחק קצר לבעלי כלבים: בנה לכלב שלך יום נכון, תראה איך הוא מגיב, ותבין למה לפעמים הוא רגוע — ולמה לפעמים הוא פשוט מתפוצץ מאנרגיה.</p>
            <div className="features">
              <div><span>⚡</span><b>בדיקת מצב יומית</b><small>מה חסר לכלב שלך — פריקה, רוגע או קשר?</small></div>
              <div><span>🎯</span><b>אתגר יומי</b><small>נסה לסיים יום עם כלב רגוע ומאוזן.</small></div>
              <div><span>🎥</span><b>שאל את שלום</b><small>הסברים קצרים מתוך מצבים אמיתיים.</small></div>
            </div>
          </div>

          <div className="form-box">
            <label>שם הכלב</label>
            <input value={dogName} onChange={(e) => setDogName(e.target.value)} placeholder="לדוגמה: לוקה" />
            <label>גזע / סוג הכלב</label>
            <input value={breed} onChange={(e) => setBreed(e.target.value)} placeholder="פודל מעורב / מלינואה / לברדור / מעורב" />
            <label>גיל הכלב</label>
            <input type="number" value={dogAge} onChange={(e) => setDogAge(e.target.value)} placeholder="בשנים" />
            <Button onClick={startGame} className="primary">בוא נראה מה הכלב שלך צריך היום</Button>
            <p className="tiny">זה לוקח פחות מדקה: מזינים כלב, מקבלים יום משחק, תגובות וטיפים לפי הבחירות שלך.</p>
          </div>
        </Card>
      </div>
    );
  }

  const taskList = [
    ["morningWalk", "טיול בוקר"],
    ["morningFood", "מזון בוקר"],
    ["mentalWork", "פריקה מנטלית"],
    ["eveningFood", "מזון ערב"],
    ["eveningWalk", "טיול ערב"],
    ["rest", "מקום / מנוחה"],
  ];

  return (
    <div dir="rtl" className="page">
      <div className="layout">
        {notification && (
          <motion.div initial={{ y: -40, opacity: 0 }} animate={{ y: 0, opacity: 1 }} className="notification">
            <div className="notif-main"><span>{notification.emoji}</span><div><b>{notification.title}</b><small>{notification.text}</small></div></div>
            <button onClick={() => setNotification(null)}>סגור</button>
          </motion.div>
        )}

        <Card className="main-card">
          <div className="top-row">
            <div><h1>{selectedDog.name} — יום {day}</h1><p>{mission}</p></div>
            <Button onClick={reset} className="outline"><RotateCcw size={16} /> התחלה מחדש</Button>
          </div>

          <div className="dog-stage">
            <motion.div key={dogMood} initial={{ scale: 0.9, opacity: 0 }} animate={{ scale: 1, opacity: 1 }} className="dog-emoji">{selectedDog.emoji}</motion.div>
            <div className="mood">{dogMood}</div>
            <motion.div key={scene.title + scene.text} initial={{ y: 12, opacity: 0, scale: 0.96 }} animate={{ y: 0, opacity: 1, scale: 1 }} transition={{ duration: 0.35 }} className="scene">
              <div className="scene-emoji">{scene.emoji}</div>
              <b>{scene.title}</b>
              <p>{scene.text}</p>
            </motion.div>
            <div className="turns">פעולות היום: {turns}/8</div>
          </div>

          <div className="actions">
            <Button onClick={() => applyAction("morning_walk")}><Footprints /> טיול בוקר</Button>
            <Button onClick={() => applyAction("long_walk")}><Footprints /> טיול ארוך / פריקה</Button>
            <Button onClick={() => applyAction("evening_walk")}><Footprints /> טיול ערב</Button>
            <Button onClick={() => applyAction("shared_play")}><Bone /> משחק משותף</Button>
            <Button onClick={() => applyAction("morning_food")}><Utensils /> מזון בוקר</Button>
            <Button onClick={() => applyAction("evening_food")}><Utensils /> מזון ערב</Button>
            <Button onClick={() => applyAction("mental_work")}><Dumbbell /> פריקה מנטלית</Button>
            <Button onClick={() => applyAction("place")}><Home /> מקום / מנוחה</Button>
          </div>

          {turns >= 8 && (
            <div className="summary">
              <div><b>סיכום יום: {balanceScore}/100</b><p>{balanceScore > 75 ? "יום מאוזן מאוד. הכלב קיבל שילוב נכון של פעילות, קשר ומנוחה." : "יש מה לשפר: נסה לאזן טוב יותר בין פריקה, אימון ומנוחה."}</p></div>
              <Button onClick={nextDay}>המשך ליום הבא</Button>
            </div>
          )}
        </Card>

        <div className="side">
          <Card>
            <h2><Heart /> מדדי הכלב</h2>
            <StatBar label="אנרגיה" value={stats.energy} />
            <StatBar label="רוגע" value={stats.calm} />
            <StatBar label="קשר עם הבעלים" value={stats.bond} />
            <StatBar label="עייפות" value={stats.tired} />
            <StatBar label="רעב" value={stats.hunger} />
            <div className="score"><small>ציון איזון נוכחי</small><b>{balanceScore}</b></div>
          </Card>

          <Card>
            <h2><Bell /> משימות היום</h2>
            <div className="tasks">
              {taskList.map(([key, label]) => (
                <div key={key} className={tasks[key] ? "task done" : "task"}><span>{tasks[key] ? "✅" : "⬜"}</span>{label}</div>
              ))}
            </div>
          </Card>

          <Card>
            <h2>כמה עזרת לכלב שלך?</h2>
            {history.map((item, index) => <div key={index} className="history">{item}</div>)}
          </Card>

          <Card>
            <h2>מושגים מקצועיים</h2>
            {professionalTerms.map((term, index) => (
              <details key={index} className="term"><summary>{term.title}</summary><p>{term.description}</p></details>
            ))}
          </Card>

          <Card className="ask">
            <h2>🐶 שאל את שלום</h2>
            <p>שאלות נפוצות עם סרטוני הסבר קצרים מתוך העולם האמיתי.</p>
            {["למה הכלב שלי לא נרגע בבית?", "מתי נכון לתת אוכל לכלב?", "למה חשוב ללמד מקום ומנוחה?"].map((q) => (
              <div key={q} className="video-card"><div><b>{q}</b><small>סרטון קצר ייפתח כאן בהמשך.</small></div><span>▶️</span></div>
            ))}
          </Card>
        </div>
      </div>
    </div>
  );
}

createRoot(document.getElementById("root")).render(<DogBalanceGame />);
