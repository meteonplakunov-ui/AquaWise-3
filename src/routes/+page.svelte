<script lang="ts">
  import { onMount, tick } from 'svelte';
  import { 
    Droplet, CheckCircle, ArrowRight, ChevronLeft, ChevronRight, 
    Info, BookOpen, Users, Mail, Globe, Bell, FileText, Camera
  } from 'lucide-svelte';

  // === НАСТРОЙКИ КАРТИНОК ===
  const ASSETS = {
    heroGarden: "/main-photo.jpg", 
    gallery: [
      "/gallery1.jpg",
      "/gallery2.jpg",
      "/gallery3.jpg",
      "/gallery4.jpg"
    ],
    logo: "/logo.png",
    mapCyprus: "/map-cyprus.png" // Не забудь добавить карту в папку static, если она нужна!
  };

  const DYNAMIC_PHRASES = {
    en: [
      "We optimize water usage through fully customizable schedules and real-time weather analysis.",
      "Making every drop count.",
      "Less waste, more growth.",
      "Anticipating conditions: systems that think one step ahead of the weather.",
      "Control watering from anywhere — protect water.",
      "Your land in the safe hands of technology.",
      "Smarter water decisions, straight from your screen."
    ],
    el: [
      "Βελτιστοποιούμε τη χρήση του νερού μέσω προσαρμόσιμων προγραμμάτων και ανάλυσης καιρού.",
      "Κάνουμε κάθε σταγόνα να μετράει",
      "Λιγότερη σπατάλη, μεγαλύτερη ανάπτυξη",
      "Προβλέποντας τις συνθήκες: συστήματα που σκέφτονται ένα βήμα μπροστά από τον καιρό.",
      "Ελέγξτε το πότισμα από οπουδήποτε — προστατέψτε το νερό",
      "Η γη σας στα ασφαλή χέρια της τεχνολογίας",
      "Εξυπνότερες αποφάσεις για το νερό, κατευθείαν από την οθόνη σας"
    ]
  };

  const translations = {
    en: {
      about: "About Us", tech: "Our Vision", journeyTitle: "Our Journey So Far",
      journeyText1: "AquaWise started as a research project at Agios Georgios Lyceum, implemented in the school garden to test smart irrigation strategies under real conditions. Initial tests focused on soil moisture monitoring, automated pump control, and rain-based adjustments.",
      journeyText2: "Since then, the system has evolved into a robust smart irrigation platform, successfully presented at multiple competitions including the Scyence Fair, Frederick-EU Conexus, and the Cyprus Final of the Stockholm Junior Water Prize 2025.",
      heroBadge: "Future of Resource Management", heroTitle: "Intelligent Irrigation", getStarted: "Get Started", howItWorksBtn: "How it works?!",
      howItWorksTitle: "How AquaWise Works?", howItWorksP1: "1. Data Collection: Moisture sensors and weather APIs collect real-time information.",
      howItWorksP2: "2. Processing: The core analyzes the data using smart algorithms.", howItWorksP3: "3. Automation: Activates water pumps only when moisture is low and no rain is predicted.",
      howItWorksP4: "4. Alerts: The user receives alerts and detailed reports.", whoWeAreTitle: "Who We Are",
      whoWeAreP1: "AquaWise is an innovative smart irrigation company developed to address water scarcity and promote sustainable resource management.",
      whoWeAreP2: "The system integrates soil moisture sensors, real-time weather data and intelligent automation to provide precise irrigation only when needed.",
      whoWeAreP3: "Our goal is to minimize water waste, enhance agricultural efficiency, and support communities in adopting data-driven water management solutions.",
      whoWeAreP4: "By combining technology and environmental insights, AquaWise ensures every drop counts.", historyCardTitle: "Our Story",
      historyCardText: "This project started with something deeply personal. One summer, I visited my grandfather's potato field. The leaves were yellow and dry. I asked him: 'Why don't you water them?' He told me: 'We don't have enough water. Not even for the roots.' A few days later, I passed by the Pedieos river near our school. What was once a thriving ecosystem was now completely dry. In Cyprus, water cuts are common, and during a recent major wildfire, people didn't even have water to help extinguish the flames. But it's not just local. Globally, agriculture uses 70% of all freshwater, yet most farms still rely on manual irrigation or simple timers.",
      historyTitle: "Local Problem, Global Solution", historyText: "Inspired by the drying bed of the Pedieos River in Cyprus, we created AquaWise to fight water scarcity.",
      missionTitle: "Our Mission", missionText: "Our mission is to transform traditional irrigation into an intelligent, data-driven process. AquaWise ensures that water is applied only when soil moisture is low and rain is not expected, maximizing efficiency and promoting sustainability.",
      missionList: ["Minimize unnecessary water consumption.", "Increase agricultural productivity.", "Support sustainable resource management.", "Enable smart, automated irrigation decisions."],
      inspirationTitle: "Inspiration of Name & Logo", inspirationText: "The name AquaWise was inspired by the idea of using water not just efficiently, but smartly. 'Aqua' represents the system's focus on sustainable water management, while 'Wise' reflects intelligent decision-making driven by real-time data and machine learning. The logo symbolizes harmony between technology and nature: a water drop with clean, modern lines representing precision and circuit-like elements to highlight the system's smart core.",
      monitoringTitle: "Production Monitoring & Threshold Adjustment System", monitoringText: "AquaWise integrates a smart Threshold Adjustment System that constantly monitors irrigation and crop production. By dynamically adjusting water levels, it ensures optimal soil moisture and maximizes crop yield, helping farmers and project managers save water while improving productivity.",
      monitoringDetails: "Users receive real-time email notifications whenever thresholds are exceeded or adjustments occur. A detailed irrigation history is recorded in CSV and HTML files, providing full traceability and analysis of past watering cycles and their impact on crop performance.",
      futureText: "Through the AquaWise website, users can live monitor system status, soil conditions, and irrigation performance, and remotely adjust thresholds for their crops or gardens. In the near future, camera integration will allow visual inspection and advanced automation, adding an extra layer of precision and control for optimal crop production.",
      partnershipsTitle: "Partnerships", partnershipsText: "AquaWise has evolved from a research prototype into a robust, market-ready smart irrigation solution, strengthened by strategic partnerships. At Agios Georgios Lyceum in Lakatamia, AquaWise has been implemented in the school garden, allowing us to test the system in a controlled educational environment. We also collaborated with the Lakatamia Municipality, deploying AquaWise in public parks. Our partnership with the Department of Meteorology (DOM) in Nicosia was essential for integrating real-time weather data. Furthermore, our collaboration with the Limassol Water Board allowed us to incorporate professional expertise in water distribution. We also worked with KOIOS, a research program at the University of Cyprus, where experts validated our concept. Moving forward, we plan to partner with local nurseries and continue our collaboration with the Stockholm Water Foundation.",
      socialTitle: "Social Media", participant: "Participant & Finalist", goBack: "Go Back"
    },
    el: {
      about: "Σχετικά με εμάς", tech: "Το Όραμά μας", journeyTitle: "Το Ταξίδι μας μέχρι τώρα",
      journeyText1: "Το AquaWise ξεκίνησε ως ένα ερευνητικό έργο στο Λύκειο Αγίου Γεωργίου, όπου εφαρμόστηκε στο σχολικό κήπο για να δοκιμάσουν έξυπνες στρατηγικές άρδευσης υπό πραγματικές συνθήκες. Οι αρχικές δοκιμές επικεντρώθηκαν στη παρακολούθηση της υγρασίας του εδάφους, στον αυτοματοποιημένο έλεγχο της αντλίας και στις προσαρμογές που βασίζονται στη βροχή.",
      journeyText2: "Εκτότε, το σύστημα έχει εξελιχθεί σε μια ισχυρή πλατφόρμα έξυπνης άρδευσης, με επιτυχία παρουσιάστηκε σε πολλούς διαγωνισμούς, συμπεριλαμβανομένων του Scyence Fair, του διαγωνισμού Frederick-EU Conexus και του Cyprus Final του Stockholm Junior Water Prize 2025.",
      heroBadge: "Το μέλλον της διαχείρισης πόρων", heroTitle: "Έξυπνη Άρδευση", getStarted: "Ξεκινήστε", howItWorksBtn: "Πώς λειτουργεί?!",
      howItWorksTitle: "Πώς λειτουργεί το AquaWise;", howItWorksP1: "1. Συλλογή Δεδομένων: Αισθητήρες υγρασίας και δεδομένα καιρού (API) συλλέγουν πληροφορίες σε πραγματικό χρόνο.",
      howItWorksP2: "2. Επεξεργασία: Το σύστημα αναλύει τα δεδομένα χρησιμοποιώντας έξυπνους αλγορίθμους.", howItWorksP3: "3. Αυτοματισμός: Ενεργοποιεί τις αντλίες νερού μόνο όταν η υγρασία είναι χαμηλή και δεν προβλέπεται βροχή.",
      howItWorksP4: "4. Ειδοποιήσεις: Ο χρήστης λαμβάνει ειδοποιήσεις και αναλυτικές αναφορές.", whoWeAreTitle: "Ποιοι Είμαστε",
      whoWeAreP1: "Το AquaWise είναι μια καινοτόμος εταιρεία έξυπνης άρδευσης που δημιουργήθηκε για να αντιμετωπίσει τη λειψυδρία και να προωθήσει τη βιώσιμη διαχείριση των πόρων.",
      whoWeAreP2: "Το σύστημα ενσωματώνει αισθητήρες υγρασίας εδάφους, δεδομένα καιρού σε πραγματικό χρόνο και έξυπνο αυτοματισμό για την παροχή ακριβούς άρδευσης μόνο όταν χρειάζεται.",
      whoWeAreP3: "Στόχος μας είναι να ελαχιστοποιήσουμε τη σπατάλη νερού, να ενισχύσουμε τη γεωργική αποδοτικότητα και να υποστηρίξουμε τις κοινότητες στην υιοθέτηση λύσεων διαχείρισης νερού με βάση τα δεδομένα.",
      whoWeAreP4: "Συνδυάζοντας την τεχνολογία με τις περιβαλλοντικές γνώσεις, το AquaWise διασφαλίζει ότι κάθε σταγόνα μετράει.", historyCardTitle: "Ιστορία",
      historyCardText: "Αυτό το έργο ξεκίνησε με κάτι βαθιά προσωπικό. Ένα καλοκαίρι, επισκέφθηκα το χωράφι πατάτας του παππού μου. Τα φύλλα ήταν κίτρινα και ξηρά. Τον ρώτησα: 'Γιατί δεν τα ποτίζεις;' Μου είπε: 'Δεν έχουμε αρκετό νερό. Ούτε για τις ρίζες.' Μερικές μέρες αργότερα, περάσα κοντά στο ποτάμι Πεδιαίος κοντά στο σχολείο μας. Αυτό που ήταν ποτέ ένα ευδοκιμούν οικοσύστημα, ήταν πλέον εντελώς ξηρό. Στην Κύπρο, οι περικοπές νερού είναι συνηθισμένες και κατά τη διάρκεια μιας πρόσφατης μεγάλης πυρκαγιάς, οι άνθρωποι δεν είχαν καν νερό για να βοηθήσουν να σβήσουν τις φλόγες. Αλλά δεν είναι μόνο τοπικό. Παγκοσμίως, η γεωργία χρησιμοποιεί το 70% όλου του γλυκού νερού, ωστόσο τα περισσότερα αγροκτήματα εξακολουθούν να βασίζονται στην χειροκίνητη άρδευση ή στα χρονόμετρα.",
      historyTitle: "Τοπικό Πρόβλημα, Παγκόσμια Λύση", historyText: "Εμπνευσμένοι από την κοίτη του ποταμού Πεδιαίου στην Κύπρο, δημιουργήσαμε το AquaWise κατά της λειψυδρίας.",
      missionTitle: "Η Αποστολή μας", missionText: "Η αποστολή μας είναι να μεταμορφώσουμε την παραδοσιακή άρδευση σε μια ευφυή διαδικασία που βασίζεται σε δεδομένα. Το AquaWise διασφαλίζει ότι το νερό εφαρμόζεται μόνο όταν η υγρασία του εδάφους είναι χαμηλή και δεν αναμένεται βροχή, μεγιστοποιώντας την απόδοση και προωθώντας τη βιωσιμότητα.",
      missionList: ["Ελαχιστοποίηση της περιττής κατανάλωσης νερού.", "Αύξηση της αγροτικής παραγωγικότητας.", "Υποστήριξη της βιώσιμης διαχείρισης πόρων.", "Ενεργοποίηση έξυπνων, αυτοματοποιημένων αποφάσεων άρδευσης."],
      inspirationTitle: "Η έμπνευση του ονόματος και του λογοτύπου μας", inspirationText: "Το όνομα AquaWise εμπνεύστηκε από την ιδέα της χρήσης νερού όχι μόνο αποδοτικά, αλλά έξυπνα. Το 'Aqua' αντιπροσωπεύει την εστίαση του συστήματος στη βιώσιμη διαχείριση νερού, ενώ το 'Wise' αντανακλά τη λήψη έξυπνων αποφάσεων η οποία προωθείται από δεδομένα και μηχανική μάθηση σε πραγματικό χρόνο. Το λογότυπο συμβολίζει την αρμονία μεταξύ τεχνολογίας και φύσης: μια σταγόνα νερού με καθαρές, σύγχρονες γραμμές που αντιπροσωπεύουν ακρίβεια και στοιχεία που μοιάζουν με ολοκληρωμένα κυκλώματα για να τονίσουν τον έξυπνο πυρήνα του συστήματος.",
      monitoringTitle: "Παρακολούθηση Παραγωγής & Σύστημα Προσαρμογής Ορίων", monitoringText: "Το AquaWise ενσωματώνει ένα έξυπνο Σύστημα Προσαρμογής Ορίων που συνεχώς παρακολουθεί την άρδευση και την παραγωγή καλλιεργειών. Προσαρμόζοντας δυναμικά τα επίπεδα νερού, διασφαλίζει τη βέλτιστη υγρασία του εδάφους και μεγιστοποιεί την απόδοση της καλλιέργειας, βοηθώντας τους αγρότες και τους διαχειριστές των έργων να εξοικονομήσουν νερό ενώ βελτιώνουν την παραγωγικότητα.",
      monitoringDetails: "Οι χρήστες λαμβάνουν ειδοποιήσεις ηλεκτρονικού ταχυδρομείου σε πραγματικό χρόνο κάθε φορά που τα όρια υπερβαίνονται ή συμβαίνουν προσαρμογές. Η λεπτομερής ιστορία άρδευσης καταγράφεται σε αρχεία CSV και HTML, παρέχοντας πλήρη δυνατότητα ανίχνευσης και ανάλυσης προηγούμενων κύκλων ποτίσματος και του αντίκτυπού τους στην απόδοση της καλλιέργειας.",
      futureText: "Μέσω του ιστότοπου AquaWise, οι χρήστες μπορούν να παρακολουθούν ζωντανά κατάσταση του συστήματος, συνθήκες εδάφους και απόδοση άρδευσης, και να προσαρμόσουν απομακρυσμένα τα όρια για τις καλλιέργειες ή τους κήπους τους. Στο εγγύς μέλλον, η ένταξη κάμερας θα επιτρέψει την οπτική επιτήρηση και την προηγμένη αυτοματοποίηση, προσθέτοντας ένα επιπλέον επίπεδο ακρίβειας και ελέγχου για τη βέλτιστη παραγωγή καλλιεργειών.",
      partnershipsTitle: "Συνεργασίες", partnershipsText: "Το AquaWise έχει εξελιχθεί από ένα πρωτότυπο έρευνας σε μια ισχυρή, έτοιμη για την αγορά λύση έξυπνης άρδευσης, ενισχυμένη από στρατηγικές συνεργασίες με πολλούς φορείς και οργανισμούς. Στο Λύκειο Αγίου Γεωργίου στη Λακατάμια, το AquaWise έχει εφαρμοστεί στο σχολικό κήπο, επιτρέποντας μας να δοκιμάσουμε το σύστημα σε ένα ελεγχόμενο εκπαιδευτικό περιβάλλον. Παράλληλα, συνεργαστήκαμε με τον Δήμο Λακατάμιας, εγκαθιστώντας το AquaWise σε δημόσια πάρκα. Η συνεργασία με το Τμήμα Μετεωρολογίας (DOM) στη Λευκωσία ήταν απαραίτητη για την ενσωμάτωση ακριβών δεδομένων. Επιπλέον, η συνεργασία μας με την Εταιρεία Ύδρευσης Λεμεσού μας επέτρεψε να ενσωματώσουμε επαγγελματική τεχνογνωσία. Συνεργαστήκαμε επίσης με το KOIOS (Πανεπιστήμιο Κύπρου), όπου ειδικοί επικύρωσαν την καινοτομία μας. Ήδη συνεργαζόμαστε με το Stockholm Water Foundation, το οποίο προωθεί την εταιρεία μας.",
      socialTitle: "Μέσα κοινωνικής δικτύωσης", participant: "Συμμετέχων & Φιναλίστ", goBack: "Επιστροφή"
    }
  };

  // === СОСТОЯНИЯ (Svelte 5) ===
  let scrolled = $state(false);
  let lang = $state<'en' | 'el'>('el');
  let page = $state<'home' | 'how-it-works'>('home');
  
  let flipPhase = $state(''); 
  let isAnimatingLang = $state(false);
  let targetLang = $state<'en' | 'el' | null>(null); 
  
  let showSplash = $state(true);
  let siteVisible = $state(false);

  let currentSlide = $state(0);
  let isSlideTransitioning = $state(false);

  let phraseIndex = $state(0);
  let phraseAnimClass = $state('');

  let t = $derived(translations[lang] || translations['en']);
  let dynamicPhrases = $derived(DYNAMIC_PHRASES[lang] || DYNAMIC_PHRASES['en']);

  // === ПЛАВНОЕ ПОЯВЛЕНИЕ ЭЛЕМЕНТОВ ===
  function reveal(node: HTMLElement, options: { delay?: number, distance?: string } = {}) {
    const delay = options.delay || 0;
    const distance = options.distance || '60px';
    
    node.classList.add('reveal-hidden');
    node.style.setProperty('--reveal-distance', distance);
    node.style.transitionDelay = `${delay}ms`;

    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        node.classList.add('reveal-visible');
        observer.disconnect();
      }
    }, { threshold: 0.15 });
    
    observer.observe(node);
    return { destroy() { observer.disconnect(); } };
  }

  onMount(() => {
    // ВАЖНОЕ ИСПРАВЛЕНИЕ СКРОЛЛА: 
    // Мы больше НЕ используем document.body.style.overflow = 'hidden'
    // Скролл всегда доступен, но сайт плавно появляется.
    window.scrollTo(0, 0);

    const handleScroll = () => { scrolled = window.scrollY > 20; };
    window.addEventListener('scroll', handleScroll);

    // Таймеры заставки
    const siteTimer = setTimeout(() => siteVisible = true, 7600);
    const subtitleTimer = setTimeout(() => {
      startSubtitleAnimation();
    }, 8300);
    const splashTimer = setTimeout(() => {
      showSplash = false;
    }, 11000);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      clearTimeout(siteTimer); clearTimeout(subtitleTimer); clearTimeout(splashTimer);
    };
  });

  function startSubtitleAnimation() {
    let count = 0;
    const phrases = dynamicPhrases;
    if (phrases.length < 2) return;

    const rapidInterval = setInterval(() => {
      phraseAnimClass = 'text-flip-out-fast';
      setTimeout(() => {
        phraseIndex = (phraseIndex + 1) % phrases.length;
        phraseAnimClass = 'text-flip-in-fast';
      }, 150);
      count++;
      
      if (count >= 10) {
        clearInterval(rapidInterval);
        setInterval(() => {
          phraseAnimClass = 'text-flip-out-slow';
          setTimeout(() => {
            let next;
            do { next = Math.floor(Math.random() * phrases.length); } while (next === phraseIndex);
            phraseIndex = next;
            phraseAnimClass = 'text-flip-in-slow';
          }, 300);
        }, 10000);
      }
    }, 300);
  }

  async function toggleLanguage() {
    if (isAnimatingLang) return; 
    const nextLang = lang === 'en' ? 'el' : 'en';
    targetLang = nextLang; 
    isAnimatingLang = true;
    flipPhase = 'out'; 

    await tick();

    setTimeout(() => {
      lang = nextLang;
      flipPhase = 'in'; 
      setTimeout(() => {
        isAnimatingLang = false;
        flipPhase = ''; 
        targetLang = null;
      }, 400); 
    }, 400); 
  }

  function nextImage() {
    if (isSlideTransitioning) return;
    isSlideTransitioning = true;
    currentSlide = (currentSlide + 1) % ASSETS.gallery.length;
    setTimeout(() => isSlideTransitioning = false, 400);
  }

  function prevImage() {
    if (isSlideTransitioning) return;
    isSlideTransitioning = true;
    currentSlide = (currentSlide - 1 + ASSETS.gallery.length) % ASSETS.gallery.length;
    setTimeout(() => isSlideTransitioning = false, 400);
  }
</script>

{#if showSplash}
  <div class="fixed inset-0 z-[200] flex items-center justify-center overflow-hidden bg-slate-900 animate-screen-dissolve pointer-events-none">
    <div class="animate-slide-in-bg bg-custom-gradient"></div>
    <div class="relative flex items-center justify-center text-6xl md:text-8xl lg:text-9xl font-black tracking-tighter z-20">
      <span class="animate-aqua-drop inline-block text-white drop-shadow-[0_0_15px_rgba(255,255,255,0.3)]">Aqua</span>
      <span class="animate-wise-rise inline-block text-cyan-400 drop-shadow-[0_0_15px_rgba(34,211,238,0.4)]">Wise</span>
      <div class="absolute -bottom-2 left-0 right-0 animate-underline-splash bg-cyan-300 rounded-full origin-center"></div>
    </div>
    <div class="absolute top-1/2 left-1/2 animate-final-drop z-50 flex items-center justify-center pointer-events-none">
      <Droplet class="text-cyan-300 w-16 h-16 fill-cyan-300 drop-shadow-[0_0_30px_cyan]" />
    </div>
    <div class="absolute top-1/2 left-1/2 rounded-full border-cyan-400 animate-ripple-ring pointer-events-none z-40 aspect-square"></div>
  </div>
{/if}

<div class="relative z-[100] min-h-screen bg-[#070f2b] font-sans text-slate-300 overflow-x-hidden">
  
  {#if page === 'how-it-works'}
    <div class="min-h-screen p-8 flex flex-col items-center justify-center text-center relative overflow-hidden bg-slate-950">
      <div class="absolute inset-0 bg-blue-500/5 blur-[100px] rounded-full pointer-events-none"></div>
      <div class="relative z-10 max-w-3xl bg-slate-900 border border-slate-800 p-10 md:p-14 rounded-[40px] shadow-[0_20px_50px_rgba(0,0,0,0.6)] animate-page-flip-in">
        <div class="w-20 h-20 bg-blue-500/10 rounded-3xl flex items-center justify-center mx-auto mb-10 border border-blue-500/20 shadow-lg shadow-blue-500/10">
          <BookOpen class="text-blue-400" size={40} />
        </div>
        <h1 class="text-4xl md:text-5xl font-black mb-10 text-white tracking-tight leading-tight">{t.howItWorksTitle}</h1>
        <div class="space-y-6 text-slate-300 text-left mb-12 leading-relaxed text-base md:text-lg bg-slate-800/40 p-8 md:p-10 rounded-3xl border border-slate-700/50 shadow-inner">
          <p class="flex items-start"><CheckCircle class="text-cyan-400 mr-4 mt-1 flex-shrink-0" size={20} /> <span>{t.howItWorksP1}</span></p>
          <p class="flex items-start"><CheckCircle class="text-cyan-400 mr-4 mt-1 flex-shrink-0" size={20} /> <span>{t.howItWorksP2}</span></p>
          <p class="flex items-start"><CheckCircle class="text-cyan-400 mr-4 mt-1 flex-shrink-0" size={20} /> <span>{t.howItWorksP3}</span></p>
          <p class="flex items-start"><CheckCircle class="text-cyan-400 mr-4 mt-1 flex-shrink-0" size={20} /> <span>{t.howItWorksP4}</span></p>
        </div>
        <button onclick={() => { page = 'home'; window.scrollTo(0,0); }} class="bg-cyan-500 text-slate-950 px-10 py-4 rounded-full font-bold hover:bg-cyan-400 transition-all flex items-center justify-center mx-auto shadow-lg shadow-cyan-500/20 text-lg active:scale-95">
          <ChevronLeft class="mr-2" size={20} /> {t.goBack}
        </button>
      </div>
    </div>
  {:else}
    
    <div class={`fixed top-6 left-0 right-0 z-50 flex justify-center px-4 md:px-6 pointer-events-none transition-all duration-1000 ease-out ${siteVisible ? 'opacity-100 translate-y-0' : 'opacity-0 -translate-y-10'}`}>
      <nav class="pointer-events-auto transition-all duration-700 ease-in-out rounded-[28px] border flex justify-between items-center px-6 py-3.5 mx-auto w-full {scrolled ? 'max-w-3xl bg-[#0b1727]/90 backdrop-blur-xl border-white/10 shadow-[0_8px_32px_rgba(0,0,0,0.5)]' : 'max-w-5xl bg-[#070f2b]/40 backdrop-blur-md border-white/5 shadow-lg'}">
        <a href="/" class="flex items-center space-x-3">
          <img src={ASSETS.logo} alt="AquaWise Logo" class="h-9 w-auto object-contain" />
        </a>
        <div class="flex items-center space-x-6">
          <div class="hidden md:flex space-x-6 text-slate-300 text-xs font-bold uppercase tracking-widest">
            <a href="#about" class="hover:text-white transition-colors">{t.about}</a>
            <a href="#vision" class="hover:text-white transition-colors">{t.tech}</a>
          </div>
          <button onclick={toggleLanguage} class="relative flex items-center space-x-2 text-white border border-white/30 px-4 py-1.5 rounded-full hover:border-white transition-all group shadow-[0_0_15px_rgba(255,255,255,0.05)] cursor-pointer overflow-hidden">
            <div class="absolute inset-0 opacity-30 group-hover:opacity-50 transition-opacity duration-500 blur-[3px] bg-cover bg-center" style="background-image: url({lang === 'en' ? 'https://flagcdn.com/w40/gb.png' : 'https://flagcdn.com/w40/gr.png'})"></div>
            <Globe size={14} class="relative z-10 group-hover:rotate-180 transition-transform duration-700 ease-in-out drop-shadow-md" />
            <span class="relative z-10 text-[11px] font-black uppercase tracking-widest mt-[1px] drop-shadow-md">{lang === 'en' ? 'EN' : 'EL'}</span>
          </button>
        </div>
      </nav>
    </div>

    {#if isAnimatingLang}
      <div class="fixed inset-0 z-[60] flex items-center justify-center pointer-events-none">
        <img src={targetLang === 'en' ? 'https://flagcdn.com/w320/gb.png' : 'https://flagcdn.com/w320/gr.png'} alt="Flag" class="w-[400px] h-[400px] object-cover rounded-full center-flag-anim" />
      </div>
    {/if}

    <div class="relative z-10 transition-transform duration-500 ease-in-out {flipPhase === 'out' ? 'page-flip-out' : flipPhase === 'in' ? 'page-flip-in' : ''}">
      
      <section class="relative pt-36 pb-20 bg-slate-950 flex items-center min-h-[85vh]">
        <div class="max-w-7xl mx-auto px-6 grid lg:grid-cols-2 gap-16 items-center w-full relative z-10">
          <div>
            <div class="transition-all duration-1000 ease-out delay-100 {siteVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-12'}">
              <div class="inline-block bg-blue-500/10 border border-blue-400/20 px-4 py-1.5 rounded-full text-cyan-400 text-xs font-bold mb-6 uppercase tracking-widest shadow-lg shadow-blue-500/5">{t.heroBadge}</div>
              <h1 class="text-5xl md:text-6xl lg:text-7xl font-black text-white mb-6 leading-tight tracking-tighter">{t.heroTitle}</h1>
            </div>
            <div class="transition-all duration-1000 ease-out delay-700 {siteVisible ? 'opacity-100 scale-100' : 'opacity-0 scale-95'}">
              <p class="text-slate-400 text-lg md:text-xl mb-10 max-w-xl leading-relaxed min-h-[3.5em] {phraseAnimClass}">{dynamicPhrases[phraseIndex]}</p>
            </div>
            <div class="flex flex-col sm:flex-row items-start sm:items-center gap-5 transition-all duration-1000 ease-out delay-1000 {siteVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8'}">
              <button class="bg-cyan-500 hover:bg-cyan-400 text-slate-950 px-8 py-4 rounded-full font-bold flex items-center transition-all text-base group shadow-[0_10px_30px_rgba(6,182,212,0.3)] hover:scale-[1.03] active:scale-95">
                {t.getStarted} <ArrowRight class="ml-2.5 w-5 h-5 transition-transform group-hover:translate-x-1.5" />
              </button>
              <button onclick={() => { page = 'how-it-works'; window.scrollTo(0,0); }} class="bg-slate-800 hover:bg-slate-700 text-white px-8 py-4 rounded-full font-bold flex items-center transition-all text-base shadow-lg shadow-black/20 group hover:scale-[1.03] active:scale-95 border border-slate-700">
                {t.howItWorksBtn} <Info class="ml-2.5 w-5 h-5 transition-transform group-hover:scale-110" />
              </button>
            </div>
          </div>
          <div class="relative group flex justify-center lg:justify-end transition-all ease-out hover:scale-[1.01] {siteVisible ? 'opacity-100 translate-x-0' : 'opacity-0 translate-x-32'}" style="transition-duration: 2500ms; transition-delay: 300ms;">
            <div class="absolute inset-0 bg-cyan-500/5 blur-[120px] rounded-full pointer-events-none"></div>
            <div class="ice-border bg-slate-900/50 backdrop-blur-2xl rounded-3xl p-3 overflow-hidden shadow-[0_30px_70px_rgba(0,0,0,0.7)] w-full max-w-3xl relative z-10 hover:border-cyan-500/40 transition-colors duration-500">
              <div class="aspect-[16/10] rounded-2xl overflow-hidden bg-slate-800 shadow-inner">
                <img src={ASSETS.heroGarden} alt="Garden View" class="w-full h-full object-cover opacity-90 group-hover:scale-105 transition-transform duration-700 ease-out" />
              </div>
            </div>
          </div>
        </div>
        <div class="absolute bottom-0 left-0 right-0 h-32 bg-gradient-to-t from-[#070f2b] to-transparent z-0"></div>
      </section>

      <section class="py-24 relative z-10 bg-[#070f2b]">
        <div use:reveal={{delay: 100}} class="max-w-5xl mx-auto px-6">
          <div class="animate-float-box">
            <div class="bg-slate-900 border border-slate-800 p-12 lg:p-16 rounded-[40px] shadow-xl hover:shadow-[0_0_50px_rgba(34,211,238,0.15)] hover:border-cyan-500/40 transition-all duration-500 text-center relative group">
              <h2 class="text-4xl md:text-5xl font-black text-white mb-10 tracking-tight leading-tight group-hover:text-cyan-400 transition-colors duration-500">{t.whoWeAreTitle}</h2>
              <div class="space-y-6 text-slate-300 leading-relaxed text-base md:text-lg relative z-10 max-w-3xl mx-auto">
                <p>{t.whoWeAreP1}</p><p>{t.whoWeAreP2}</p><p>{t.whoWeAreP3}</p>
                <p class="font-bold text-cyan-400 pt-6 tracking-wide text-lg md:text-xl bg-slate-800/50 inline-block px-6 py-2 rounded-full border border-slate-700 mt-4 group-hover:bg-slate-800 transition-colors duration-500">{t.whoWeAreP4}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="about" class="py-28 relative z-10 bg-[#070f2b] border-t border-slate-800/50">
        <div use:reveal={{distance: '80px'}} class="max-w-7xl mx-auto px-6 grid lg:grid-cols-5 gap-16 items-center">
          
          <div class="lg:col-span-3 relative w-full max-w-3xl mx-auto aspect-[16/10] rounded-3xl overflow-hidden shadow-[0_25px_60px_rgba(0,0,0,0.4)] hover:scale-[1.02] hover:shadow-[0_0_40px_rgba(34,211,238,0.2)] transition-all duration-500 group ice-border p-2 bg-slate-900/60 backdrop-blur-sm hover:border-cyan-500/50">
            <div class="w-full h-full rounded-2xl overflow-hidden relative shadow-inner bg-slate-800">
                <div class="absolute inset-0 w-full h-full transition-all ease-in-out {isSlideTransitioning ? 'blur-lg scale-110 opacity-60' : 'blur-0 scale-100 opacity-100'}" style="transition-duration: {isSlideTransitioning ? '200ms' : '600ms'}">
                    <img src={ASSETS.gallery[currentSlide]} class="w-full h-full object-cover" alt="Gallery Image" />
                </div>
            </div>
            <div class="absolute inset-0 flex items-center justify-between px-6 z-40 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
              <button onclick={prevImage} class="p-3 rounded-full bg-slate-950/60 backdrop-blur-md text-white hover:bg-cyan-500 hover:text-black transition-all shadow-xl active:scale-90 border border-slate-700"><ChevronLeft size={22} /></button>
              <button onclick={nextImage} class="p-3 rounded-full bg-slate-950/60 backdrop-blur-md text-white hover:bg-cyan-500 hover:text-black transition-all shadow-xl active:scale-90 border border-slate-700"><ChevronRight size={22} /></button>
            </div>
            <div class="absolute bottom-5 left-1/2 -translate-x-1/2 flex gap-2 z-40 bg-slate-950/50 px-3 py-1.5 rounded-full backdrop-blur-sm border border-slate-700">
                {#each ASSETS.gallery as _, i}
                    <button onclick={() => { if(!isSlideTransitioning) { isSlideTransitioning=true; currentSlide=i; setTimeout(()=>isSlideTransitioning=false, 400); } }} class="h-2.5 rounded-full transition-all duration-300 {currentSlide === i ? 'bg-cyan-400 w-6' : 'bg-slate-600 w-2.5 hover:bg-slate-400'}"></button>
                {/each}
            </div>
          </div>

          <div class="lg:col-span-2 space-y-6">
            <div class="inline-flex items-center space-x-2.5 bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 px-4 py-1.5 rounded-full shadow-lg shadow-emerald-500/5">
              <CheckCircle size={16} /><span class="text-xs font-bold uppercase tracking-wider mt-0.5">{t.participant}</span>
            </div>
            <h2 class="text-4xl md:text-5xl font-black text-white leading-tight tracking-tight">{t.historyTitle}</h2>
            <p class="text-base md:text-lg text-slate-400 leading-relaxed">{t.historyText}</p>
            <div class="p-6 bg-slate-900 border border-slate-800 border-l-4 border-l-cyan-400 rounded-r-2xl italic text-sm md:text-base text-slate-300 shadow-xl relative overflow-hidden">
                <div class="absolute -right-4 -bottom-6 text-slate-800 opacity-30"><Droplet size={80}/></div>
                <p class="relative z-10">"Inspired by Cypriot resilience, developed for global sustainability."</p>
            </div>
          </div>
        </div>
      </section>

      <section id="vision" class="py-28 bg-slate-950 text-white relative border-t border-slate-800/50">
        <div class="max-w-7xl mx-auto px-6 relative z-10">
          
          <div use:reveal={{distance: '60px', delay: 100}} class="grid lg:grid-cols-2 gap-10 mb-10 items-stretch">
            <div class="bg-slate-900 border border-slate-800 p-10 lg:p-12 rounded-[32px] shadow-xl hover:shadow-[0_0_40px_rgba(59,130,246,0.2)] hover:border-blue-500/40 transition-all duration-500 group flex flex-col justify-center">
              <div class="flex items-center space-x-4 mb-10">
                <div class="p-3.5 bg-blue-500/10 rounded-2xl border border-blue-500/30 group-hover:bg-blue-500/20 transition-colors duration-500"><Globe class="text-blue-400" size={28} /></div>
                <h2 class="text-4xl font-black tracking-tight text-white group-hover:text-blue-400 transition-colors duration-500 leading-tight">{t.journeyTitle}</h2>
              </div>
              <div class="space-y-6 text-slate-300 leading-relaxed text-base md:text-lg">
                <p>{t.journeyText1}</p>
                <p class="border-l-2 border-cyan-500 pl-6 italic text-slate-400 bg-slate-800/50 py-4 rounded-r-xl">{t.journeyText2}</p>
              </div>
            </div>
            
            <div class="bg-slate-900 border border-slate-800 rounded-[32px] overflow-hidden flex items-center justify-center min-h-[400px] shadow-xl hover:shadow-[0_0_40px_rgba(59,130,246,0.15)] hover:border-blue-500/30 transition-all duration-500 group relative">
                {#if ASSETS.mapCyprus}
                    <img src={ASSETS.mapCyprus} alt="Cyprus Map" class="absolute inset-0 w-full h-full object-cover opacity-30 group-hover:opacity-50 group-hover:scale-105 transition-all duration-1000 ease-out" />
                {/if}
                <div class="relative z-10 w-full h-full p-10 flex items-center justify-center">
                  <svg viewBox="0 0 500 300" class="w-full h-auto max-w-xl opacity-90 drop-shadow-[0_0_20px_rgba(56,189,248,0.3)]">
                    <polygon points="40,150 110,120 130,210 50,230" fill="#1e293b" stroke="#334155" stroke-width="1"/>
                    <polygon points="190,110 310,80 330,240 220,230" fill="#1e293b" stroke="#334155" stroke-width="1"/>
                    <polygon points="370,120 470,130 450,260 350,270" fill="#1e293b" stroke="#334155" stroke-width="1"/>
                    <path d="M 60,180 Q 150,130 250,160 T 400,160 Q 420,160 430,190" fill="none" stroke="#0ea5e9" stroke-width="2.5" stroke-dasharray="6,6" class="animate-pulse"/>
                    <circle cx="100" cy="160" r="5" fill="#38bdf8" stroke="#fff" stroke-width="1.5"/>
                    <circle cx="270" cy="155" r="5" fill="#38bdf8" stroke="#fff" stroke-width="1.5"/>
                    <circle cx="430" cy="190" r="6" fill="#0ea5e9" stroke="#fff" stroke-width="2" class="animate-pulse" />
                    <text x="415" y="218" fill="#fff" font-size="14" font-weight="900" class="uppercase tracking-widest drop-shadow-lg">Cyprus</text>
                  </svg>
                </div>
            </div>
          </div>
          
          <div class="grid md:grid-cols-2 gap-10">
              
              <div use:reveal={{delay: 200}} class="md:col-span-2 bg-slate-900 border border-slate-800 p-10 lg:p-12 rounded-[32px] shadow-xl hover:shadow-[0_0_45px_rgba(245,158,11,0.15)] hover:border-amber-500/40 transition-all duration-500 group flex flex-col">
                <div class="flex items-center space-x-4 mb-10">
                  <div class="w-14 h-14 bg-amber-500/10 rounded-2xl flex items-center justify-center border border-amber-500/30 group-hover:bg-amber-500/20 transition-colors duration-500"><BookOpen class="w-7 h-7 text-amber-400" /></div>
                  <span class="text-sm font-bold tracking-widest uppercase text-amber-400 pt-0.5">{t.historyCardTitle}</span>
                </div>
                <h2 class="text-4xl md:text-5xl font-black mb-10 leading-tight tracking-tight text-white group-hover:text-amber-400 transition-colors duration-500">{t.historyCardTitle}</h2>
                <div class="columns-1 lg:columns-2 gap-12 text-base md:text-lg text-slate-300 leading-relaxed space-y-5 bg-slate-800/40 p-8 rounded-2xl border border-slate-700/50">
                  <p>{t.historyCardText}</p>
                </div>
              </div>

              <div use:reveal={{delay: 300}} class="bg-slate-900 border border-slate-800 p-10 rounded-[32px] shadow-xl hover:shadow-[0_0_40px_rgba(16,185,129,0.15)] hover:border-emerald-500/40 transition-all duration-500 group flex flex-col">
                <div class="flex items-center space-x-4 mb-9">
                    <div class="p-3.5 bg-emerald-500/10 rounded-2xl border border-emerald-500/30 group-hover:bg-emerald-500/20 transition-colors duration-500"><CheckCircle class="text-emerald-400" size={26} /></div>
                    <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-emerald-400 transition-colors duration-500 tracking-tight">{t.missionTitle}</h2>
                </div>
                <p class="text-base md:text-lg text-slate-300 leading-relaxed mb-8">{t.missionText}</p>
                <ul class="space-y-4 text-base md:text-lg text-slate-300 bg-slate-800/50 p-6 rounded-2xl border border-slate-700 flex-grow">
                    {#each t.missionList as item}
                        <li class="flex items-start gap-3.5"><Droplet class="text-cyan-400 mt-1 flex-shrink-0" size={18} /><span>{item}</span></li>
                    {/each}
                </ul>
              </div>

              <div use:reveal={{delay: 400}} class="bg-slate-900 border border-slate-800 p-10 rounded-[32px] shadow-xl hover:shadow-[0_0_40px_rgba(34,211,238,0.15)] hover:border-cyan-500/40 transition-all duration-500 group flex flex-col">
                <div class="flex items-center space-x-4 mb-9">
                    <div class="p-3.5 bg-cyan-500/10 rounded-2xl border border-cyan-500/30 group-hover:bg-cyan-500/20 transition-colors duration-500"><Droplet class="text-cyan-400" size={26} /></div>
                    <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-cyan-400 transition-colors duration-500 tracking-tight">{t.inspirationTitle}</h2>
                </div>
                <div class="text-base md:text-lg text-slate-300 leading-relaxed space-y-5 bg-slate-800/50 p-7 rounded-2xl border border-slate-700 flex-grow">
                    <p>{t.inspirationText}</p>
                    <img src={ASSETS.logo} alt="AquaWise Logo Large" class="h-16 mx-auto pt-6 group-hover:scale-105 transition-transform duration-500" />
                </div>
              </div>

              <div use:reveal={{delay: 500}} class="md:col-span-2 bg-slate-900 border border-slate-800 p-10 lg:p-12 rounded-[32px] shadow-xl hover:shadow-[0_0_45px_rgba(34,211,238,0.2)] hover:border-cyan-500/40 transition-all duration-500 grid md:grid-cols-5 gap-10 items-center group">
                <div class="md:col-span-3 space-y-6">
                    <div class="flex items-center space-x-4 mb-9">
                        <div class="p-3.5 bg-cyan-500/10 rounded-2xl border border-cyan-500/30 group-hover:bg-cyan-500/20 transition-colors duration-500"><Bell class="text-cyan-400" size={26} /></div>
                        <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-cyan-400 transition-colors duration-500 tracking-tight leading-tight">{t.monitoringTitle}</h2>
                    </div>
                    <p class="text-base md:text-lg text-slate-300 leading-relaxed">{t.monitoringText}</p>
                    <p class="text-base md:text-lg text-slate-300 leading-relaxed bg-slate-800/50 p-6 rounded-2xl border border-slate-700 italic border-l-4 border-l-cyan-400">{t.monitoringDetails}</p>
                </div>
                <div class="md:col-span-2 relative h-full min-h-[200px] flex items-center justify-center border-2 border-slate-700/50 rounded-2xl p-6 bg-slate-950/50 group-hover:border-cyan-500/40 transition-colors duration-500">
                    <div class="flex items-end gap-3 h-32 w-full max-w-[200px] justify-center overflow-hidden">
                        <div class="w-8 bg-slate-700 rounded-t-md animate-data-bar" style="animation-delay: 0.1s"></div>
                        <div class="w-8 bg-cyan-600 rounded-t-md animate-data-bar" style="animation-delay: 0.3s"></div>
                        <div class="w-8 bg-blue-500 rounded-t-md animate-data-bar" style="animation-delay: 0.5s"></div>
                        <div class="w-8 bg-cyan-400 rounded-t-md animate-data-bar" style="animation-delay: 0.2s"></div>
                        <div class="w-8 bg-emerald-500 rounded-t-md animate-data-bar" style="animation-delay: 0.4s"></div>
                    </div>
                    <Bell class="absolute top-4 right-4 text-cyan-400 animate-pulse" size={24} />
                </div>
              </div>

              <div use:reveal={{delay: 600}} class="bg-slate-900 border border-slate-800 p-10 rounded-[32px] shadow-xl hover:shadow-[0_0_40px_rgba(59,130,246,0.15)] hover:border-blue-500/40 transition-all duration-500 flex flex-col group relative overflow-hidden">
                {#if ASSETS.mapCyprus}
                    <img src={ASSETS.mapCyprus} alt="Background Map" class="absolute inset-0 w-full h-full object-cover opacity-5 group-hover:opacity-10 group-hover:scale-105 transition-all duration-1000 pointer-events-none" />
                {/if}
                <div class="flex items-center space-x-4 mb-9 relative z-10">
                    <div class="p-3.5 bg-blue-500/10 rounded-2xl border border-blue-500/30 group-hover:bg-blue-500/20 transition-colors duration-500"><Camera class="text-blue-400" size={26} /></div>
                    <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-blue-400 transition-colors duration-500 tracking-tight">{t.about}</h2>
                </div>
                <div class="text-base md:text-lg text-slate-300 leading-relaxed space-y-5 relative z-10 bg-slate-800/50 p-7 rounded-2xl border border-slate-700 flex-grow flex items-center justify-center">
                    <p class="text-center">{t.futureText}</p>
                </div>
              </div>

              <div use:reveal={{delay: 700}} class="bg-slate-900 border border-slate-800 p-10 rounded-[32px] shadow-xl hover:shadow-[0_0_40px_rgba(16,185,129,0.15)] hover:border-emerald-500/40 transition-all duration-500 flex flex-col group">
                <div class="flex items-center space-x-4 mb-9">
                    <div class="p-3.5 bg-emerald-500/10 rounded-2xl border border-emerald-500/30 group-hover:bg-emerald-500/20 transition-colors duration-500"><FileText class="text-emerald-400" size={26} /></div>
                    <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-emerald-400 transition-colors duration-500 tracking-tight">Cyprus Final</h2>
                </div>
                <div class="text-base md:text-lg text-slate-300 leading-relaxed space-y-5 bg-slate-800/50 p-7 rounded-2xl border border-slate-700 flex-grow flex items-center justify-center relative">
                    <Droplet class="text-cyan-400 animate-bounce absolute -top-5 left-1/2 -translate-x-1/2" size={40}/>
                    <p class="text-center font-bold text-white text-xl md:text-2xl pt-4">Stockholm Junior Water Prize 2025</p>
                </div>
              </div>

              <div use:reveal={{delay: 800}} class="md:col-span-2 bg-slate-900 border border-slate-800 p-10 lg:p-12 rounded-[32px] shadow-xl hover:shadow-[0_0_45px_rgba(99,102,241,0.15)] hover:border-indigo-500/40 transition-all duration-500 group">
                <div class="flex items-center space-x-4 mb-10">
                    <div class="p-3.5 bg-indigo-500/10 rounded-2xl border border-indigo-500/30 group-hover:bg-indigo-500/20 transition-colors duration-500"><Users class="text-indigo-400" size={26} /></div>
                    <h2 class="text-3xl md:text-4xl font-black text-white group-hover:text-indigo-400 transition-colors duration-500 tracking-tight leading-tight">{t.partnershipsTitle}</h2>
                </div>
                <div class="columns-1 lg:columns-2 gap-12 text-base md:text-lg text-slate-300 leading-relaxed bg-slate-800/40 p-8 rounded-2xl border border-slate-700/50">
                   <p>{t.partnershipsText}</p>
                </div>
              </div>

          </div>
        </div>
      </section>

      <section class="py-32 bg-slate-900 border-t border-slate-800/50 relative z-10 flex flex-col items-center justify-center overflow-hidden">
        <div use:reveal class="max-w-7xl w-full mx-auto px-6 text-center relative">
          <h2 class="text-sm font-bold text-cyan-500 mb-16 tracking-[0.3em] uppercase">Interactive Pipeline (Coming Soon)</h2>
          <div class="absolute top-[60%] left-0 w-full h-2 bg-slate-800 -translate-y-1/2 z-0"></div>
          <div class="grid grid-cols-4 gap-8 relative z-10">
            {#each [1, 2, 3, 4] as num}
              <div class="aspect-video bg-slate-800/80 border-2 border-dashed border-slate-600 rounded-2xl flex items-center justify-center backdrop-blur-sm hover:scale-105 hover:border-cyan-400 transition-all duration-300 cursor-pointer group">
                <span class="text-slate-500 font-bold group-hover:text-cyan-400 transition-colors">Image {num}</span>
              </div>
            {/each}
          </div>
          <p class="mt-16 text-slate-400 font-medium max-w-2xl mx-auto text-lg">
            Здесь появится анимация: при прокрутке экрана вода будет течь по трубе сквозь эти 4 картинки.
          </p>
        </div>
      </section>

      <footer class="bg-slate-950 py-16 relative z-10 border-t border-slate-800/50">
        <div use:reveal class="max-w-7xl mx-auto px-6">
          <div class="flex flex-col md:flex-row justify-between items-center gap-12 mb-16">
            <div class="flex flex-col items-center md:items-start space-y-3">
              <img src={ASSETS.logo} alt="AquaWise Logo" class="h-12 w-auto object-contain hover:scale-105 transition-transform" />
              <p class="text-slate-600 text-xs uppercase tracking-widest font-bold bg-slate-800/50 px-3 py-1 rounded-full border border-slate-700">SJWP FINALIST 2025</p>
            </div>
            
            <div class="flex flex-col md:flex-row items-center gap-6 md:gap-10 text-slate-400">
              <h3 class="text-sm font-bold uppercase tracking-widest text-slate-500">{t.socialTitle}:</h3>
              <a href="mailto:info@aquawise.cy" class="flex items-center space-x-2.5 hover:text-white hover:bg-slate-800 transition-all group bg-slate-900 px-5 py-2.5 rounded-full border border-slate-800 shadow-md">
                <Mail size={18} class="text-blue-400 group-hover:text-cyan-400" /> <span class="text-base">info@aquawise.cy</span>
              </a>
              <a href="https://aquawise.cy" target="_blank" rel="noopener noreferrer" class="flex items-center space-x-2.5 hover:text-white hover:bg-slate-800 transition-all group bg-slate-900 px-5 py-2.5 rounded-full border border-slate-800 shadow-md">
                <Globe size={18} class="text-cyan-400 group-hover:text-blue-400" /> <span class="text-base">aquawise.cy</span>
              </a>
            </div>
          </div>
          
          <div class="border-t border-slate-800/50 pt-10 flex flex-col sm:flex-row justify-between items-center gap-6 text-center sm:text-left">
            <div class="text-slate-600 text-xs font-mono tracking-tighter uppercase bg-slate-900 px-4 py-1 rounded-full border border-slate-800">© 2025 CYPRUS — SMART IRRIGATION CORE</div>
            <div class="text-slate-700 text-xs">Developed with SvelteKit & Tailwind v4</div>
          </div>
        </div>
      </footer>
      
    </div>
  {/if}
</div>

<style>
  :global(html) { scroll-behavior: smooth; }
  :global(body) { background-color: #070f2b; }

  :global(.ice-border) { 
    border: 2px solid rgba(56, 189, 248, 0.25); 
    box-shadow: 0 0 30px rgba(56, 189, 248, 0.1), inset 0 0 15px rgba(56, 189, 248, 0.05); 
  }
  
  @keyframes floatBox { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
  :global(.animate-float-box) { animation: floatBox 6s ease-in-out infinite; }

  @keyframes dataBar { 0%, 100% { height: 25%; opacity: 0.5; } 50% { height: 100%; opacity: 1; } }
  :global(.animate-data-bar) { animation: dataBar 1.5s ease-in-out infinite; height: 25%; will-change: height, opacity; }

  @keyframes pageFlipOut { 0% { transform: perspective(2000px) rotateX(0deg) scale(1); opacity: 1; filter: blur(0px); } 100% { transform: perspective(2000px) rotateX(8deg) scale(0.97); opacity: 0; filter: blur(5px); } }
  @keyframes pageFlipIn { 0% { transform: perspective(2000px) rotateX(-8deg) scale(0.97); opacity: 0; filter: blur(5px); } 100% { transform: perspective(2000px) rotateX(0deg) scale(1); opacity: 1; filter: blur(0px); } }
  :global(.page-flip-out) { animation: pageFlipOut 0.4s ease-in-out forwards; transform-origin: center top; backface-visibility: hidden; }
  :global(.page-flip-in) { animation: pageFlipIn 0.5s ease-out forwards; transform-origin: center top; backface-visibility: hidden; }
  :global(.animate-page-flip-in) { animation: pageFlipIn 0.6s ease-out both; transform-origin: center top; }

  @keyframes textFlipOutFast { 0% { transform: perspective(1000px) rotateX(0deg); opacity: 1; } 100% { transform: perspective(1000px) rotateX(90deg); opacity: 0; } }
  @keyframes textFlipInFast { 0% { transform: perspective(1000px) rotateX(-90deg); opacity: 0; } 100% { transform: perspective(1000px) rotateX(0deg); opacity: 1; } }
  :global(.text-flip-out-fast) { animation: textFlipOutFast 0.15s ease-in forwards; backface-visibility: hidden; }
  :global(.text-flip-in-fast) { animation: textFlipInFast 0.15s ease-out forwards; backface-visibility: hidden; }

  @keyframes textFlipOutSlow { 0% { transform: perspective(1000px) rotateX(0deg); opacity: 1; filter: blur(0px); } 100% { transform: perspective(1000px) rotateX(90deg); opacity: 0; filter: blur(2px); } }
  @keyframes textFlipInSlow { 0% { transform: perspective(1000px) rotateX(-90deg); opacity: 0; filter: blur(2px); } 100% { transform: perspective(1000px) rotateX(0deg); opacity: 1; filter: blur(0px); } }
  :global(.text-flip-out-slow) { animation: textFlipOutSlow 0.4s ease-in forwards; backface-visibility: hidden; }
  :global(.text-flip-in-slow) { animation: textFlipInSlow 0.5s ease-out forwards; backface-visibility: hidden; }

  @keyframes centerFlagFlash { 0% { transform: scale(0.5); opacity: 0; filter: blur(20px); } 40% { transform: scale(1.2); opacity: 0.5; filter: blur(40px); } 100% { transform: scale(1.6); opacity: 0; filter: blur(60px); } }
  :global(.center-flag-anim) { animation: centerFlagFlash 1s cubic-bezier(0.4, 0, 0.2, 1) forwards; }

  @keyframes aqua-drop-anim { 0% { transform: translateY(-100vh) scale(0.8); opacity: 0; } 30% { opacity: 1; } 100% { transform: translateY(0) scale(1); opacity: 1; } }
  :global(.animate-aqua-drop) { animation: aqua-drop-anim 4s cubic-bezier(0.22, 1, 0.36, 1) 2.0s both; }

  @keyframes wise-rise-anim { 0% { transform: translateY(100vh) scale(0.8); opacity: 0; } 30% { opacity: 1; } 100% { transform: translateY(0) scale(1); opacity: 1; } }
  :global(.animate-wise-rise) { animation: wise-rise-anim 4s cubic-bezier(0.22, 1, 0.36, 1) 2.0s both; }

  @keyframes underline-splash-anim { 0% { transform: scaleX(0); opacity: 0; height: 1px; } 20% { transform: scaleX(1.05); opacity: 1; height: 7px; filter: drop-shadow(0 0 25px #22d3ee); } 100% { transform: scaleX(1); opacity: 0.9; height: 4px; filter: drop-shadow(0 0 12px #22d3ee); } }
  :global(.animate-underline-splash) { animation: underline-splash-anim 1.4s cubic-bezier(0.1, 0.9, 0.2, 1) 6.0s both; transform-origin: center; }

  @keyframes final-drop-anim { 0% { transform: translate(-50%, -100vh) scale(1); opacity: 1; filter: blur(0px); } 70% { transform: translate(-50%, -50%) scale(1.3); opacity: 1; filter: blur(0px); } 100% { transform: translate(-50%, -50%) scale(6); opacity: 0; filter: blur(15px); } }
  :global(.animate-final-drop) { animation: final-drop-anim 0.7s ease-in 7.0s both; pointer-events: none; }

  @keyframes slide-in-bg { 0% { transform: translateX(110vw); } 100% { transform: translateX(-10vw); } }
  :global(.animate-slide-in-bg) { width: 150vw; height: 120vh; top: -10vh; left: 0; position: absolute; animation: slide-in-bg 3s cubic-bezier(0.25, 1, 0.3, 1) 0.5s both; will-change: transform; }

  @keyframes ripple-ring { 0% { width: 0; height: 0; opacity: 1; border-width: 12px; transform: translate(-50%, -50%); } 100% { width: 300vw; height: 300vw; opacity: 0; border-width: 1px; transform: translate(-50%, -50%); } }
  :global(.animate-ripple-ring) { opacity: 0; animation: ripple-ring 2s cubic-bezier(0.4, 0, 0.2, 1) 7.6s forwards; will-change: width, height, opacity; }

  @keyframes screen-dissolve-anim { 0% { opacity: 1; filter: blur(0); } 100% { opacity: 0; filter: blur(30px); visibility: hidden; } }
  :global(.animate-screen-dissolve) { animation: screen-dissolve-anim 2.8s ease-out 7.6s both; pointer-events: none; will-change: opacity, blur; }

  :global(.bg-custom-gradient) {
    background: linear-gradient(135deg, rgba(6, 29, 213, 0.5), rgba(14, 63, 197, 0.55), rgba(21, 109, 179, 0.55), rgba(28, 164, 159, 0.55), rgba(18, 141, 63, 0.5), rgba(10, 116, 32, 0.5), rgba(2, 93, 6, 0.4));
    backdrop-filter: blur(30px); -webkit-backdrop-filter: blur(30px);
  }
  
  :global(.reveal-hidden) {
    opacity: 0;
    transform: translateY(var(--reveal-distance, 60px));
    transition: opacity 1s ease-out, transform 1s cubic-bezier(0.22, 1, 0.36, 1);
    will-change: opacity, transform;
  }
  :global(.reveal-visible) {
    opacity: 1;
    transform: translateY(0);
  }
</style>