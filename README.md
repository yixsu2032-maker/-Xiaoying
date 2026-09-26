import React, { useState, useMemo } from "react";
import { createRoot } from "react-dom/client";

const LANGUAGES: Record<string, { name: string; label: string; search: string; copy: string; copied: string; allCats: string; title: string; subtitle: string; about: string; nav: string; aboutTitle: string; aboutDesc: string; feat1Title: string; feat1Desc: string; feat2Title: string; feat2Desc: string; feat3Title: string; feat3Desc: string; feat4Title: string; feat4Desc: string; feat5Title: string; feat5Desc: string; usageTitle: string; usage1: string; usage2: string; usage3: string; usage4: string; cats: Record<string, string>; brandResults: string; noResults: string; noResultsSub: string; clearSearch: string }> = {
  zh: { name: "中文", label: "语言", search: "搜索应用或品牌...", copy: "复制链接", copied: "已复制!", allCats: "全部", title: "应用链接导航", subtitle: "一键复制，快速访问全球热门应用", about: "关于", nav: "导航", aboutTitle: "关于应用链接导航", aboutDesc: "应用链接导航是一个免费的全球应用链接聚合平台，帮助用户快速找到并复制250多款热门应用的官方网址。无论你是想访问社交媒体、AI工具、游戏平台还是购物网站，只需一键即可复制链接，省去手动输入的麻烦。", feat1Title: "一键复制", feat1Desc: "点击「复制链接」按钮，官方网址立即复制到剪贴板，粘贴即用。", feat2Title: "分类浏览", feat2Desc: "12大分类涵盖社交、AI、游戏、购物、金融等，快速定位你需要的应用。", feat3Title: "多语言支持", feat3Desc: "支持20种语言界面，满足全球用户的使用需求。", feat4Title: "主题切换", feat4Desc: "8种精美主题，深色、浅色及多种色调，随心切换护眼模式。", feat5Title: "实时搜索", feat5Desc: "搜索框即时过滤，输入应用名称或网址关键词秒速定位。", usageTitle: "使用场景", usage1: "在受限网络环境下快速获取应用官网地址", usage2: "向朋友分享某款应用的官方链接", usage3: "一站式查找全球主流应用入口", usage4: "快速对比同类应用的官网", brandResults: "品牌相关应用", noResults: "未找到相关结果", noResultsSub: "没有找到与", clearSearch: "清除搜索", cats: { Social: "社交", Messaging: "通讯", Video: "视频", Music: "音乐", AI: "人工智能", Games: "游戏", Shopping: "购物", Finance: "金融", Productivity: "效率", News: "新闻", Travel: "旅行", Health: "健康" } },
  en: { name: "English", label: "Language", search: "Search apps or brands...", copy: "Copy Link", copied: "Copied!", allCats: "All", title: "App Link Navigator", subtitle: "One-click copy, quick access to global apps", about: "About", nav: "Navigator", aboutTitle: "About App Link Navigator", aboutDesc: "App Link Navigator is a free global app link aggregator that helps users quickly find and copy the official URLs of 250+ popular apps. Whether you want to access social media, AI tools, gaming platforms, or shopping sites — copy the link in one click, no manual typing needed.", feat1Title: "One-Click Copy", feat1Desc: "Click the Copy Link button and the official URL is instantly copied to your clipboard, ready to paste.", feat2Title: "Category Browse", feat2Desc: "12 categories covering Social, AI, Games, Shopping, Finance and more — find what you need fast.", feat3Title: "Multi-Language", feat3Desc: "20 language interfaces to serve users around the world.", feat4Title: "Theme Switching", feat4Desc: "8 beautiful themes — dark, light, and colorful — switch anytime for eye comfort.", feat5Title: "Live Search", feat5Desc: "The search box filters instantly as you type an app name or URL keyword.", usageTitle: "Use Cases", usage1: "Quickly get official app URLs in restricted network environments", usage2: "Share an app's official link with friends", usage3: "One-stop directory for global mainstream apps", usage4: "Quickly compare official sites of similar apps", brandResults: "Brand Apps", noResults: "No results found", noResultsSub: "No apps matched", clearSearch: "Clear Search", cats: { Social: "Social", Messaging: "Messaging", Video: "Video", Music: "Music", AI: "AI", Games: "Games", Shopping: "Shopping", Finance: "Finance", Productivity: "Productivity", News: "News", Travel: "Travel", Health: "Health" } },
  es: { name: "Español", label: "Idioma", search: "Buscar apps o marcas...", copy: "Copiar enlace", copied: "¡Copiado!", allCats: "Todo", title: "Navegador de Aplicaciones", subtitle: "Copia con un clic, acceso rápido a apps globales", about: "Acerca de", nav: "Navegador", aboutTitle: "Acerca del Navegador de Aplicaciones", aboutDesc: "El Navegador de Aplicaciones es una plataforma gratuita que agrega enlaces de más de 250 apps populares para que puedas copiarlos con un clic.", feat1Title: "Copia con un clic", feat1Desc: "Haz clic en Copiar enlace y la URL oficial se copia al portapapeles al instante.", feat2Title: "Explorar por categoría", feat2Desc: "12 categorías: Social, IA, Juegos, Compras, Finanzas y más.", feat3Title: "Multiidioma", feat3Desc: "20 idiomas disponibles para usuarios de todo el mundo.", feat4Title: "Cambio de tema", feat4Desc: "8 temas hermosos — oscuro, claro y coloridos.", feat5Title: "Búsqueda en vivo", feat5Desc: "El cuadro de búsqueda filtra al instante mientras escribes.", usageTitle: "Casos de uso", usage1: "Obtener URLs oficiales en entornos de red restringidos", usage2: "Compartir el enlace oficial de una app con amigos", usage3: "Directorio único de apps globales", usage4: "Comparar sitios oficiales de apps similares", brandResults: "Apps de la marca", noResults: "Sin resultados", noResultsSub: "No se encontraron apps para", clearSearch: "Limpiar búsqueda", cats: { Social: "Social", Messaging: "Mensajería", Video: "Video", Music: "Música", AI: "IA", Games: "Juegos", Shopping: "Compras", Finance: "Finanzas", Productivity: "Productividad", News: "Noticias", Travel: "Viajes", Health: "Salud" } },
  fr: { name: "Français", label: "Langue", search: "Rechercher apps ou marques...", copy: "Copier le lien", copied: "Copié!", allCats: "Tout", title: "Navigateur d'Applications", subtitle: "Copiez en un clic, accès rapide aux apps mondiales", about: "À propos", nav: "Navigateur", aboutTitle: "À propos du Navigateur d'Applications", aboutDesc: "Le Navigateur d'Applications est une plateforme gratuite qui agrège les liens de plus de 250 apps populaires pour une copie en un clic.", feat1Title: "Copie en un clic", feat1Desc: "Cliquez sur Copier le lien et l'URL officielle est copiée dans le presse-papiers.", feat2Title: "Navigation par catégorie", feat2Desc: "12 catégories : Social, IA, Jeux, Shopping, Finance et plus.", feat3Title: "Multilingue", feat3Desc: "20 langues disponibles pour les utilisateurs du monde entier.", feat4Title: "Changement de thème", feat4Desc: "8 thèmes magnifiques — sombre, clair et colorés.", feat5Title: "Recherche en direct", feat5Desc: "La barre de recherche filtre instantanément pendant la saisie.", usageTitle: "Cas d'utilisation", usage1: "Obtenir des URLs officielles dans des environnements réseau restreints", usage2: "Partager le lien officiel d'une app avec des amis", usage3: "Répertoire unique des apps mondiales", usage4: "Comparer les sites officiels d'apps similaires", brandResults: "Apps de la marque", noResults: "Aucun résultat", noResultsSub: "Aucune app trouvée pour", clearSearch: "Effacer la recherche", cats: { Social: "Social", Messaging: "Messagerie", Video: "Vidéo", Music: "Musique", AI: "IA", Games: "Jeux", Shopping: "Shopping", Finance: "Finance", Productivity: "Productivité", News: "Actualités", Travel: "Voyage", Health: "Santé" } },
  de: { name: "Deutsch", label: "Sprache", search: "Apps oder Marken suchen...", copy: "Link kopieren", copied: "Kopiert!", allCats: "Alle", title: "App-Link-Navigator", subtitle: "Ein-Klick-Kopie, schneller Zugriff auf globale Apps", about: "Über uns", nav: "Navigator", aboutTitle: "Über den App-Link-Navigator", aboutDesc: "Der App-Link-Navigator ist eine kostenlose Plattform, die Links von über 250 beliebten Apps aggregiert, damit Sie sie mit einem Klick kopieren können.", feat1Title: "Ein-Klick-Kopie", feat1Desc: "Klicken Sie auf Link kopieren und die offizielle URL wird sofort in die Zwischenablage kopiert.", feat2Title: "Kategorien", feat2Desc: "12 Kategorien: Social, KI, Spiele, Shopping, Finanzen und mehr.", feat3Title: "Mehrsprachig", feat3Desc: "20 Sprachen für Nutzer weltweit.", feat4Title: "Themenwechsel", feat4Desc: "8 schöne Themen — dunkel, hell und bunt.", feat5Title: "Live-Suche", feat5Desc: "Das Suchfeld filtert sofort während der Eingabe.", usageTitle: "Anwendungsfälle", usage1: "Offizielle App-URLs in eingeschränkten Netzwerken abrufen", usage2: "Den offiziellen Link einer App mit Freunden teilen", usage3: "Einziges Verzeichnis globaler Apps", usage4: "Offizielle Seiten ähnlicher Apps vergleichen", brandResults: "Marken-Apps", noResults: "Keine Ergebnisse", noResultsSub: "Keine Apps gefunden für", clearSearch: "Suche löschen", cats: { Social: "Sozial", Messaging: "Nachrichten", Video: "Video", Music: "Musik", AI: "KI", Games: "Spiele", Shopping: "Einkaufen", Finance: "Finanzen", Productivity: "Produktivität", News: "Nachrichten", Travel: "Reisen", Health: "Gesundheit" } },
  ja: { name: "日本語", label: "言語", search: "アプリやブランドを検索...", copy: "リンクをコピー", copied: "コピー済み!", allCats: "すべて", title: "アプリリンクナビ", subtitle: "ワンクリックコピー、グローバルアプリへの素早いアクセス", about: "概要", nav: "ナビ", aboutTitle: "アプリリンクナビについて", aboutDesc: "アプリリンクナビは、250以上の人気アプリの公式URLをワンクリックでコピーできる無料プラットフォームです。", feat1Title: "ワンクリックコピー", feat1Desc: "リンクをコピーボタンをクリックすると、公式URLが即座にクリップボードにコピーされます。", feat2Title: "カテゴリ閲覧", feat2Desc: "ソーシャル、AI、ゲーム、ショッピングなど12カテゴリ。", feat3Title: "多言語対応", feat3Desc: "世界中のユーザー向けに20言語に対応。", feat4Title: "テーマ切替", feat4Desc: "ダーク、ライトなど8つの美しいテーマ。", feat5Title: "リアルタイム検索", feat5Desc: "入力しながら即座にフィルタリング。", usageTitle: "使用シーン", usage1: "制限されたネットワーク環境で公式URLを取得", usage2: "アプリの公式リンクを友人と共有", usage3: "グローバルアプリの一元ディレクトリ", usage4: "類似アプリの公式サイトを比較", brandResults: "ブランドのアプリ", noResults: "結果が見つかりません", noResultsSub: "一致するアプリなし:", clearSearch: "検索をクリア", cats: { Social: "ソーシャル", Messaging: "メッセージ", Video: "動画", Music: "音楽", AI: "AI", Games: "ゲーム", Shopping: "ショッピング", Finance: "金融", Productivity: "生産性", News: "ニュース", Travel: "旅行", Health: "健康" } },
  ko: { name: "한국어", label: "언어", search: "앱 또는 브랜드 검색...", copy: "링크 복사", copied: "복사됨!", allCats: "전체", title: "앱 링크 네비게이터", subtitle: "원클릭 복사, 글로벌 앱 빠른 접근", about: "소개", nav: "네비게이터", aboutTitle: "앱 링크 네비게이터 소개", aboutDesc: "앱 링크 네비게이터는 250개 이상의 인기 앱 공식 URL을 원클릭으로 복사할 수 있는 무료 플랫폼입니다.", feat1Title: "원클릭 복사", feat1Desc: "링크 복사 버튼을 클릭하면 공식 URL이 즉시 클립보드에 복사됩니다.", feat2Title: "카테고리 탐색", feat2Desc: "소셜, AI, 게임, 쇼핑 등 12개 카테고리.", feat3Title: "다국어 지원", feat3Desc: "전 세계 사용자를 위한 20개 언어 지원.", feat4Title: "테마 전환", feat4Desc: "다크, 라이트 등 8가지 아름다운 테마.", feat5Title: "실시간 검색", feat5Desc: "입력하는 동안 즉시 필터링.", usageTitle: "사용 사례", usage1: "제한된 네트워크 환경에서 공식 URL 획득", usage2: "앱 공식 링크를 친구와 공유", usage3: "글로벌 앱 원스톱 디렉토리", usage4: "유사 앱 공식 사이트 비교", brandResults: "브랜드 앱", noResults: "결과 없음", noResultsSub: "검색 결과 없음:", clearSearch: "검색 지우기", cats: { Social: "소셜", Messaging: "메시지", Video: "동영상", Music: "음악", AI: "AI", Games: "게임", Shopping: "쇼핑", Finance: "금융", Productivity: "생산성", News: "뉴스", Travel: "여행", Health: "건강" } },
  pt: { name: "Português", label: "Idioma", search: "Pesquisar apps ou marcas...", copy: "Copiar link", copied: "Copiado!", allCats: "Tudo", title: "Navegador de Apps", subtitle: "Copie com um clique, acesso rápido a apps globais", about: "Sobre", nav: "Navegador", aboutTitle: "Sobre o Navegador de Apps", aboutDesc: "O Navegador de Apps é uma plataforma gratuita que agrega links de mais de 250 apps populares para cópia com um clique.", feat1Title: "Cópia com um clique", feat1Desc: "Clique em Copiar link e a URL oficial é copiada para a área de transferência.", feat2Title: "Navegar por categoria", feat2Desc: "12 categorias: Social, IA, Jogos, Compras, Finanças e mais.", feat3Title: "Multilíngue", feat3Desc: "20 idiomas para usuários do mundo todo.", feat4Title: "Troca de tema", feat4Desc: "8 temas bonitos — escuro, claro e coloridos.", feat5Title: "Pesquisa ao vivo", feat5Desc: "A caixa de pesquisa filtra instantaneamente enquanto você digita.", usageTitle: "Casos de uso", usage1: "Obter URLs oficiais em ambientes de rede restritos", usage2: "Compartilhar o link oficial de um app com amigos", usage3: "Diretório único de apps globais", usage4: "Comparar sites oficiais de apps similares", brandResults: "Apps da marca", noResults: "Nenhum resultado", noResultsSub: "Nenhum app encontrado para", clearSearch: "Limpar pesquisa", cats: { Social: "Social", Messaging: "Mensagens", Video: "Vídeo", Music: "Música", AI: "IA", Games: "Jogos", Shopping: "Compras", Finance: "Finanças", Productivity: "Produtividade", News: "Notícias", Travel: "Viagens", Health: "Saúde" } },
  ru: { name: "Русский", label: "Язык", search: "Поиск приложений или брендов...", copy: "Копировать ссылку", copied: "Скопировано!", allCats: "Все", title: "Навигатор приложений", subtitle: "Копирование в один клик, быстрый доступ к приложениям", about: "О нас", nav: "Навигатор", aboutTitle: "О навигаторе приложений", aboutDesc: "Навигатор приложений — бесплатная платформа, агрегирующая ссылки более 250 популярных приложений для копирования в один клик.", feat1Title: "Копирование в один клик", feat1Desc: "Нажмите кнопку копирования — официальный URL мгновенно попадёт в буфер обмена.", feat2Title: "Просмотр по категориям", feat2Desc: "12 категорий: Соцсети, ИИ, Игры, Покупки, Финансы и другие.", feat3Title: "Многоязычность", feat3Desc: "20 языков интерфейса для пользователей по всему миру.", feat4Title: "Смена темы", feat4Desc: "8 красивых тем — тёмная, светлая и цветные.", feat5Title: "Живой поиск", feat5Desc: "Поле поиска фильтрует мгновенно по мере ввода.", usageTitle: "Сценарии использования", usage1: "Быстро получить официальные URL в ограниченных сетях", usage2: "Поделиться официальной ссылкой приложения с друзьями", usage3: "Единый каталог глобальных приложений", usage4: "Сравнить официальные сайты похожих приложений", brandResults: "Приложения бренда", noResults: "Ничего не найдено", noResultsSub: "Нет приложений для", clearSearch: "Очистить поиск", cats: { Social: "Соцсети", Messaging: "Мессенджеры", Video: "Видео", Music: "Музыка", AI: "ИИ", Games: "Игры", Shopping: "Покупки", Finance: "Финансы", Productivity: "Продуктивность", News: "Новости", Travel: "Путешествия", Health: "Здоровье" } },
  ar: { name: "العربية", label: "اللغة", search: "البحث عن التطبيقات أو العلامات...", copy: "نسخ الرابط", copied: "تم النسخ!", allCats: "الكل", title: "دليل روابط التطبيقات", subtitle: "نسخ بنقرة واحدة، وصول سريع للتطبيقات العالمية", about: "حول", nav: "الدليل", aboutTitle: "حول دليل روابط التطبيقات", aboutDesc: "دليل روابط التطبيقات منصة مجانية تجمع روابط أكثر من 250 تطبيق شهير لنسخها بنقرة واحدة.", feat1Title: "نسخ بنقرة واحدة", feat1Desc: "انقر على نسخ الرابط وسيُنسخ الرابط الرسمي فوراً إلى الحافظة.", feat2Title: "تصفح حسب الفئة", feat2Desc: "12 فئة: اجتماعي، ذكاء اصطناعي، ألعاب، تسوق، مالية والمزيد.", feat3Title: "متعدد اللغات", feat3Desc: "20 لغة لخدمة المستخدمين حول العالم.", feat4Title: "تبديل السمة", feat4Desc: "8 سمات جميلة — داكنة وفاتحة وملونة.", feat5Title: "بحث فوري", feat5Desc: "يُصفّي مربع البحث فوراً أثناء الكتابة.", usageTitle: "حالات الاستخدام", usage1: "الحصول على روابط رسمية في بيئات الشبكات المقيدة", usage2: "مشاركة الرابط الرسمي لتطبيق مع الأصدقاء", usage3: "دليل شامل للتطبيقات العالمية", usage4: "مقارنة المواقع الرسمية للتطبيقات المتشابهة", brandResults: "تطبيقات العلامة", noResults: "لا توجد نتائج", noResultsSub: "لم يتم العثور على تطبيقات لـ", clearSearch: "مسح البحث", cats: { Social: "اجتماعي", Messaging: "مراسلة", Video: "فيديو", Music: "موسيقى", AI: "ذكاء اصطناعي", Games: "ألعاب", Shopping: "تسوق", Finance: "مالية", Productivity: "إنتاجية", News: "أخبار", Travel: "سفر", Health: "صحة" } },
  hi: { name: "हिन्दी", label: "भाषा", search: "ऐप या ब्रांड खोजें...", copy: "लिंक कॉपी करें", copied: "कॉपी हो गया!", allCats: "सभी", title: "ऐप लिंक नेविगेटर", subtitle: "एक क्लिक में कॉपी करें, वैश्विक ऐप्स तक त्वरित पहुंच", about: "परिचय", nav: "नेविगेटर", aboutTitle: "ऐप लिंक नेविगेटर के बारे में", aboutDesc: "ऐप लिंक नेविगेटर एक मुफ्त प्लेटफॉर्म है जो 250+ लोकप्रिय ऐप्स के आधिकारिक URL को एक क्लिक में कॉपी करने की सुविधा देता है।", feat1Title: "एक क्लिक कॉपी", feat1Desc: "लिंक कॉपी करें बटन दबाएं और आधिकारिक URL तुरंत क्लिपबोर्ड में आ जाएगा।", feat2Title: "श्रेणी ब्राउज़", feat2Desc: "12 श्रेणियां: सोशल, AI, गेम्स, शॉपिंग, फाइनेंस आदि।", feat3Title: "बहुभाषी", feat3Desc: "दुनिया भर के उपयोगकर्ताओं के लिए 20 भाषाएं।", feat4Title: "थीम बदलें", feat4Desc: "8 सुंदर थीम — डार्क, लाइट और रंगीन।", feat5Title: "लाइव सर्च", feat5Desc: "टाइप करते समय तुरंत फ़िल्टर होता है।", usageTitle: "उपयोग के मामले", usage1: "प्रतिबंधित नेटवर्क में आधिकारिक URL प्राप्त करें", usage2: "दोस्तों के साथ ऐप का आधिकारिक लिंक साझा करें", usage3: "वैश्विक ऐप्स की एकल निर्देशिका", usage4: "समान ऐप्स की आधिकारिक साइटों की तुलना करें", brandResults: "ब्रांड ऐप्स", noResults: "कोई परिणाम नहीं", noResultsSub: "कोई ऐप नहीं मिला:", clearSearch: "खोज साफ़ करें", cats: { Social: "सोशल", Messaging: "मैसेजिंग", Video: "वीडियो", Music: "संगीत", AI: "AI", Games: "गेम्स", Shopping: "शॉपिंग", Finance: "वित्त", Productivity: "उत्पादकता", News: "समाचार", Travel: "यात्रा", Health: "स्वास्थ्य" } },
  it: { name: "Italiano", label: "Lingua", search: "Cerca app o marchi...", copy: "Copia link", copied: "Copiato!", allCats: "Tutto", title: "Navigatore App", subtitle: "Copia con un clic, accesso rapido alle app globali", about: "Info", nav: "Navigatore", aboutTitle: "Informazioni sul Navigatore App", aboutDesc: "Il Navigatore App è una piattaforma gratuita che aggrega i link di oltre 250 app popolari per copiarli con un clic.", feat1Title: "Copia con un clic", feat1Desc: "Clicca su Copia link e l'URL ufficiale viene copiato negli appunti.", feat2Title: "Sfoglia per categoria", feat2Desc: "12 categorie: Social, IA, Giochi, Shopping, Finanza e altro.", feat3Title: "Multilingua", feat3Desc: "20 lingue per utenti di tutto il mondo.", feat4Title: "Cambio tema", feat4Desc: "8 bellissimi temi — scuro, chiaro e colorati.", feat5Title: "Ricerca live", feat5Desc: "La casella di ricerca filtra istantaneamente mentre digiti.", usageTitle: "Casi d'uso", usage1: "Ottenere URL ufficiali in ambienti di rete limitati", usage2: "Condividere il link ufficiale di un'app con gli amici", usage3: "Directory unica delle app globali", usage4: "Confrontare i siti ufficiali di app simili", brandResults: "App del marchio", noResults: "Nessun risultato", noResultsSub: "Nessuna app trovata per", clearSearch: "Cancella ricerca", cats: { Social: "Social", Messaging: "Messaggistica", Video: "Video", Music: "Musica", AI: "IA", Games: "Giochi", Shopping: "Shopping", Finance: "Finanza", Productivity: "Produttività", News: "Notizie", Travel: "Viaggi", Health: "Salute" } },
  tr: { name: "Türkçe", label: "Dil", search: "Uygulama veya marka ara...", copy: "Bağlantıyı kopyala", copied: "Kopyalandı!", allCats: "Tümü", title: "Uygulama Bağlantı Gezgini", subtitle: "Tek tıkla kopyala, küresel uygulamalara hızlı erişim", about: "Hakkında", nav: "Gezgin", aboutTitle: "Uygulama Bağlantı Gezgini Hakkında", aboutDesc: "Uygulama Bağlantı Gezgini, 250'den fazla popüler uygulamanın resmi URL'lerini tek tıkla kopyalamanızı sağlayan ücretsiz bir platformdur.", feat1Title: "Tek tıkla kopyala", feat1Desc: "Bağlantıyı kopyala düğmesine tıklayın, resmi URL anında panoya kopyalanır.", feat2Title: "Kategoriye göre gözat", feat2Desc: "12 kategori: Sosyal, Yapay Zeka, Oyunlar, Alışveriş, Finans ve daha fazlası.", feat3Title: "Çok dilli", feat3Desc: "Dünya genelindeki kullanıcılar için 20 dil.", feat4Title: "Tema değiştirme", feat4Desc: "8 güzel tema — koyu, açık ve renkli.", feat5Title: "Canlı arama", feat5Desc: "Arama kutusu yazarken anında filtreler.", usageTitle: "Kullanım senaryoları", usage1: "Kısıtlı ağ ortamlarında resmi URL'leri alma", usage2: "Bir uygulamanın resmi bağlantısını arkadaşlarla paylaşma", usage3: "Küresel uygulamalar için tek dizin", usage4: "Benzer uygulamaların resmi sitelerini karşılaştırma", brandResults: "Marka Uygulamaları", noResults: "Sonuç bulunamadı", noResultsSub: "Uygulama bulunamadı:", clearSearch: "Aramayı temizle", cats: { Social: "Sosyal", Messaging: "Mesajlaşma", Video: "Video", Music: "Müzik", AI: "Yapay Zeka", Games: "Oyunlar", Shopping: "Alışveriş", Finance: "Finans", Productivity: "Verimlilik", News: "Haberler", Travel: "Seyahat", Health: "Sağlık" } },
  nl: { name: "Nederlands", label: "Taal", search: "Apps of merken zoeken...", copy: "Link kopiëren", copied: "Gekopieerd!", allCats: "Alles", title: "App Link Navigator", subtitle: "Kopieer met één klik, snelle toegang tot wereldwijde apps", about: "Over", nav: "Navigator", aboutTitle: "Over de App Link Navigator", aboutDesc: "De App Link Navigator is een gratis platform dat links van meer dan 250 populaire apps aggregeert voor kopiëren met één klik.", feat1Title: "Één klik kopiëren", feat1Desc: "Klik op Link kopiëren en de officiële URL wordt direct naar het klembord gekopieerd.", feat2Title: "Bladeren per categorie", feat2Desc: "12 categorieën: Sociaal, AI, Games, Winkelen, Financiën en meer.", feat3Title: "Meertalig", feat3Desc: "20 talen voor gebruikers wereldwijd.", feat4Title: "Thema wisselen", feat4Desc: "8 mooie thema's — donker, licht en kleurrijk.", feat5Title: "Live zoeken", feat5Desc: "Het zoekvak filtert direct terwijl u typt.", usageTitle: "Gebruiksscenario's", usage1: "Officiële URL's ophalen in beperkte netwerkomgevingen", usage2: "De officiële link van een app delen met vrienden", usage3: "Eén-stop directory voor wereldwijde apps", usage4: "Officiële sites van vergelijkbare apps vergelijken", brandResults: "Merk-apps", noResults: "Geen resultaten", noResultsSub: "Geen apps gevonden voor", clearSearch: "Zoekopdracht wissen", cats: { Social: "Sociaal", Messaging: "Berichten", Video: "Video", Music: "Muziek", AI: "AI", Games: "Games", Shopping: "Winkelen", Finance: "Financiën", Productivity: "Productiviteit", News: "Nieuws", Travel: "Reizen", Health: "Gezondheid" } },
  pl: { name: "Polski", label: "Język", search: "Szukaj aplikacji lub marek...", copy: "Kopiuj link", copied: "Skopiowano!", allCats: "Wszystko", title: "Nawigator Linków Aplikacji", subtitle: "Kopiuj jednym kliknięciem, szybki dostęp do globalnych aplikacji", about: "O nas", nav: "Nawigator", aboutTitle: "O Nawigatorze Linków Aplikacji", aboutDesc: "Nawigator Linków Aplikacji to darmowa platforma agregująca linki ponad 250 popularnych aplikacji do kopiowania jednym kliknięciem.", feat1Title: "Kopiowanie jednym kliknięciem", feat1Desc: "Kliknij Kopiuj link, a oficjalny URL zostanie natychmiast skopiowany do schowka.", feat2Title: "Przeglądaj według kategorii", feat2Desc: "12 kategorii: Społecznościowe, AI, Gry, Zakupy, Finanse i więcej.", feat3Title: "Wielojęzyczny", feat3Desc: "20 języków dla użytkowników na całym świecie.", feat4Title: "Zmiana motywu", feat4Desc: "8 pięknych motywów — ciemny, jasny i kolorowe.", feat5Title: "Wyszukiwanie na żywo", feat5Desc: "Pole wyszukiwania filtruje natychmiast podczas pisania.", usageTitle: "Przypadki użycia", usage1: "Uzyskiwanie oficjalnych URL w ograniczonych środowiskach sieciowych", usage2: "Udostępnianie oficjalnego linku aplikacji znajomym", usage3: "Jeden katalog globalnych aplikacji", usage4: "Porównywanie oficjalnych stron podobnych aplikacji", brandResults: "Aplikacje marki", noResults: "Brak wyników", noResultsSub: "Nie znaleziono aplikacji dla", clearSearch: "Wyczyść wyszukiwanie", cats: { Social: "Społecznościowe", Messaging: "Wiadomości", Video: "Wideo", Music: "Muzyka", AI: "AI", Games: "Gry", Shopping: "Zakupy", Finance: "Finanse", Productivity: "Produktywność", News: "Wiadomości", Travel: "Podróże", Health: "Zdrowie" } },
  vi: { name: "Tiếng Việt", label: "Ngôn ngữ", search: "Tìm kiếm ứng dụng hoặc thương hiệu...", copy: "Sao chép liên kết", copied: "Đã sao chép!", allCats: "Tất cả", title: "Điều hướng liên kết ứng dụng", subtitle: "Sao chép một cú nhấp, truy cập nhanh ứng dụng toàn cầu", about: "Giới thiệu", nav: "Điều hướng", aboutTitle: "Giới thiệu về Điều hướng liên kết ứng dụng", aboutDesc: "Điều hướng liên kết ứng dụng là nền tảng miễn phí tổng hợp liên kết của hơn 250 ứng dụng phổ biến để sao chép chỉ với một cú nhấp.", feat1Title: "Sao chép một cú nhấp", feat1Desc: "Nhấp Sao chép liên kết và URL chính thức được sao chép ngay vào clipboard.", feat2Title: "Duyệt theo danh mục", feat2Desc: "12 danh mục: Mạng xã hội, AI, Trò chơi, Mua sắm, Tài chính và hơn thế.", feat3Title: "Đa ngôn ngữ", feat3Desc: "20 ngôn ngữ cho người dùng trên toàn thế giới.", feat4Title: "Chuyển đổi chủ đề", feat4Desc: "8 chủ đề đẹp — tối, sáng và nhiều màu sắc.", feat5Title: "Tìm kiếm trực tiếp", feat5Desc: "Hộp tìm kiếm lọc ngay lập tức khi bạn gõ.", usageTitle: "Trường hợp sử dụng", usage1: "Lấy URL chính thức trong môi trường mạng bị hạn chế", usage2: "Chia sẻ liên kết chính thức của ứng dụng với bạn bè", usage3: "Thư mục một cửa cho các ứng dụng toàn cầu", usage4: "So sánh các trang chính thức của các ứng dụng tương tự", brandResults: "Ứng dụng thương hiệu", noResults: "Không tìm thấy kết quả", noResultsSub: "Không có ứng dụng nào cho", clearSearch: "Xóa tìm kiếm", cats: { Social: "Mạng xã hội", Messaging: "Nhắn tin", Video: "Video", Music: "Âm nhạc", AI: "AI", Games: "Trò chơi", Shopping: "Mua sắm", Finance: "Tài chính", Productivity: "Năng suất", News: "Tin tức", Travel: "Du lịch", Health: "Sức khỏe" } },
  th: { name: "ภาษาไทย", label: "ภาษา", search: "ค้นหาแอปหรือแบรนด์...", copy: "คัดลอกลิงก์", copied: "คัดลอกแล้ว!", allCats: "ทั้งหมด", title: "นำทางลิงก์แอป", subtitle: "คัดลอกด้วยคลิกเดียว เข้าถึงแอปทั่วโลกได้อย่างรวดเร็ว", about: "เกี่ยวกับ", nav: "นำทาง", aboutTitle: "เกี่ยวกับนำทางลิงก์แอป", aboutDesc: "นำทางลิงก์แอปเป็นแพลตฟอร์มฟรีที่รวบรวมลิงก์ของแอปยอดนิยมกว่า 250 แอปเพื่อคัดลอกด้วยคลิกเดียว", feat1Title: "คัดลอกด้วยคลิกเดียว", feat1Desc: "คลิกคัดลอกลิงก์และ URL อย่างเป็นทางการจะถูกคัดลอกไปยังคลิปบอร์ดทันที", feat2Title: "เรียกดูตามหมวดหมู่", feat2Desc: "12 หมวดหมู่: โซเชียล, AI, เกม, ช้อปปิ้ง, การเงิน และอื่นๆ", feat3Title: "หลายภาษา", feat3Desc: "20 ภาษาสำหรับผู้ใช้ทั่วโลก", feat4Title: "เปลี่ยนธีม", feat4Desc: "8 ธีมสวยงาม — มืด สว่าง และหลากสี", feat5Title: "ค้นหาสด", feat5Desc: "กล่องค้นหากรองทันทีขณะพิมพ์", usageTitle: "กรณีการใช้งาน", usage1: "รับ URL อย่างเป็นทางการในสภาพแวดล้อมเครือข่ายที่จำกัด", usage2: "แชร์ลิงก์อย่างเป็นทางการของแอปกับเพื่อน", usage3: "ไดเรกทอรีเดียวสำหรับแอปทั่วโลก", usage4: "เปรียบเทียบเว็บไซต์อย่างเป็นทางการของแอปที่คล้ายกัน", brandResults: "แอปของแบรนด์", noResults: "ไม่พบผลลัพธ์", noResultsSub: "ไม่พบแอปสำหรับ", clearSearch: "ล้างการค้นหา", cats: { Social: "โซเชียล", Messaging: "ข้อความ", Video: "วิดีโอ", Music: "เพลง", AI: "AI", Games: "เกม", Shopping: "ช้อปปิ้ง", Finance: "การเงิน", Productivity: "ประสิทธิภาพ", News: "ข่าว", Travel: "ท่องเที่ยว", Health: "สุขภาพ" } },
  id: { name: "Bahasa Indonesia", label: "Bahasa", search: "Cari aplikasi atau merek...", copy: "Salin tautan", copied: "Disalin!", allCats: "Semua", title: "Navigator Tautan Aplikasi", subtitle: "Salin satu klik, akses cepat ke aplikasi global", about: "Tentang", nav: "Navigator", aboutTitle: "Tentang Navigator Tautan Aplikasi", aboutDesc: "Navigator Tautan Aplikasi adalah platform gratis yang mengumpulkan tautan lebih dari 250 aplikasi populer untuk disalin dengan satu klik.", feat1Title: "Salin satu klik", feat1Desc: "Klik Salin tautan dan URL resmi langsung disalin ke clipboard.", feat2Title: "Jelajahi per kategori", feat2Desc: "12 kategori: Sosial, AI, Game, Belanja, Keuangan dan lainnya.", feat3Title: "Multibahasa", feat3Desc: "20 bahasa untuk pengguna di seluruh dunia.", feat4Title: "Ganti tema", feat4Desc: "8 tema indah — gelap, terang, dan berwarna.", feat5Title: "Pencarian langsung", feat5Desc: "Kotak pencarian memfilter secara instan saat Anda mengetik.", usageTitle: "Kasus penggunaan", usage1: "Mendapatkan URL resmi di lingkungan jaringan terbatas", usage2: "Berbagi tautan resmi aplikasi dengan teman", usage3: "Direktori satu atap untuk aplikasi global", usage4: "Membandingkan situs resmi aplikasi serupa", brandResults: "Aplikasi merek", noResults: "Tidak ada hasil", noResultsSub: "Tidak ada aplikasi untuk", clearSearch: "Hapus pencarian", cats: { Social: "Sosial", Messaging: "Pesan", Video: "Video", Music: "Musik", AI: "AI", Games: "Game", Shopping: "Belanja", Finance: "Keuangan", Productivity: "Produktivitas", News: "Berita", Travel: "Perjalanan", Health: "Kesehatan" } },
  ms: { name: "Bahasa Melayu", label: "Bahasa", search: "Cari aplikasi atau jenama...", copy: "Salin pautan", copied: "Disalin!", allCats: "Semua", title: "Navigator Pautan Aplikasi", subtitle: "Salin satu klik, akses pantas ke aplikasi global", about: "Tentang", nav: "Navigator", aboutTitle: "Tentang Navigator Pautan Aplikasi", aboutDesc: "Navigator Pautan Aplikasi ialah platform percuma yang mengumpulkan pautan lebih 250 aplikasi popular untuk disalin dengan satu klik.", feat1Title: "Salin satu klik", feat1Desc: "Klik Salin pautan dan URL rasmi terus disalin ke papan klip.", feat2Title: "Layari mengikut kategori", feat2Desc: "12 kategori: Sosial, AI, Permainan, Membeli-belah, Kewangan dan lain-lain.", feat3Title: "Pelbagai bahasa", feat3Desc: "20 bahasa untuk pengguna di seluruh dunia.", feat4Title: "Tukar tema", feat4Desc: "8 tema cantik — gelap, cerah dan berwarna-warni.", feat5Title: "Carian langsung", feat5Desc: "Kotak carian menapis serta-merta semasa anda menaip.", usageTitle: "Kes penggunaan", usage1: "Mendapatkan URL rasmi dalam persekitaran rangkaian terhad", usage2: "Berkongsi pautan rasmi aplikasi dengan rakan", usage3: "Direktori sehenti untuk aplikasi global", usage4: "Membandingkan laman web rasmi aplikasi yang serupa", brandResults: "Aplikasi jenama", noResults: "Tiada keputusan", noResultsSub: "Tiada aplikasi untuk", clearSearch: "Kosongkan carian", cats: { Social: "Sosial", Messaging: "Pesanan", Video: "Video", Music: "Muzik", AI: "AI", Games: "Permainan", Shopping: "Membeli-belah", Finance: "Kewangan", Productivity: "Produktiviti", News: "Berita", Travel: "Pelancongan", Health: "Kesihatan" } },
  sv: { name: "Svenska", label: "Språk", search: "Sök appar eller varumärken...", copy: "Kopiera länk", copied: "Kopierat!", allCats: "Alla", title: "App-länknavigator", subtitle: "Kopiera med ett klick, snabb åtkomst till globala appar", about: "Om", nav: "Navigator", aboutTitle: "Om App-länknavigatorn", aboutDesc: "App-länknavigatorn är en gratis plattform som samlar länkar till över 250 populära appar för kopiering med ett klick.", feat1Title: "Kopiera med ett klick", feat1Desc: "Klicka på Kopiera länk så kopieras den officiella URL:en direkt till urklipp.", feat2Title: "Bläddra efter kategori", feat2Desc: "12 kategorier: Socialt, AI, Spel, Shopping, Finans med mera.", feat3Title: "Flerspråkig", feat3Desc: "20 språk för användare världen över.", feat4Title: "Byt tema", feat4Desc: "8 vackra teman — mörkt, ljust och färgglada.", feat5Title: "Live-sökning", feat5Desc: "Sökrutan filtrerar omedelbart medan du skriver.", usageTitle: "Användningsfall", usage1: "Hämta officiella URL:er i begränsade nätverksmiljöer", usage2: "Dela en apps officiella länk med vänner", usage3: "En katalog för globala appar", usage4: "Jämföra officiella webbplatser för liknande appar", brandResults: "Varumärkesappar", noResults: "Inga resultat", noResultsSub: "Inga appar hittades för", clearSearch: "Rensa sökning", cats: { Social: "Socialt", Messaging: "Meddelanden", Video: "Video", Music: "Musik", AI: "AI", Games: "Spel", Shopping: "Shopping", Finance: "Finans", Productivity: "Produktivitet", News: "Nyheter", Travel: "Resor", Health: "Hälsa" } },
};

const CATEGORIES = ["Social", "Messaging", "Video", "Music", "AI", "Games", "Shopping", "Finance", "Productivity", "News", "Travel", "Health"];

// Brand → app name aliases for smart search
const BRAND_ALIASES: Record<string, string[]> = {
  "google": [
    "YouTube",
    "Google Drive",
    "Google Meet",
    "YouTube Music",
    "Gemini",
    "Fitbit"
  ],
  "apple": [
    "Apple Music",
    "Apple TV+"
  ],
  "microsoft": [
    "Teams",
    "Copilot",
    "GitHub",
    "Xbox",
    "Minecraft",
    "LinkedIn",
    "Blizzard"
  ],
  "meta": [
    "Facebook",
    "Instagram",
    "WhatsApp",
    "Threads",
    "Messenger"
  ],
  "tencent": [
    "微信 WeChat",
    "企业微信 WeCom",
    "QQ",
    "QQ邮箱",
    "腾讯会议 VooV Meeting",
    "腾讯视频",
    "腾讯元宝",
    "QQ音乐",
    "酷狗音乐",
    "酷我音乐",
    "全民K歌",
    "王者荣耀",
    "和平精英",
    "腾讯文档",
    "腾讯新闻",
    "微信支付"
  ],
  "bytedance": [
    "TikTok",
    "抖音 Douyin",
    "西瓜视频",
    "豆包 Doubao",
    "即梦AI",
    "汽水音乐",
    "飞书 Feishu",
    "剪映",
    "今日头条"
  ],
  "amazon": [
    "Amazon",
    "Prime Video",
    "Amazon Music",
    "Twitch"
  ],
  "netflix": [
    "Netflix"
  ],
  "spotify": [
    "Spotify"
  ],
  "valve": [
    "Steam"
  ],
  "epic": [
    "Epic Games",
    "Fortnite"
  ],
  "riot": [
    "Valorant",
    "League of Legends"
  ],
  "sony": [
    "PlayStation"
  ],
  "nintendo": [
    "Nintendo"
  ],
  "atlassian": [
    "Jira",
    "Confluence",
    "Trello"
  ],
  "openai": [
    "ChatGPT"
  ],
  "anthropic": [
    "Claude"
  ],
  "twitter": [
    "Twitter / X"
  ],
  "snap": [
    "Snapchat"
  ],
  "paypal": [
    "PayPal",
    "Venmo"
  ],
  "coinbase": [
    "Coinbase"
  ],
  "binance": [
    "Binance"
  ],
  "uber": [
    "Uber"
  ],
  "airbnb": [
    "Airbnb"
  ],
  "booking": [
    "Booking.com"
  ],
  "shopee": [
    "Shopee"
  ],
  "baidu": [
    "百度贴吧",
    "文心助手",
    "百度网盘",
    "百度地图",
    "爱奇艺 iQIYI"
  ],
  "line": [
    "Line"
  ],
  "discord": [
    "Discord"
  ],
  "notion": [
    "Notion"
  ],
  "figma": [
    "Figma"
  ],
  "canva": [
    "Canva"
  ],
  "github": [
    "GitHub"
  ],
  "dropbox": [
    "Dropbox"
  ],
  "asana": [
    "Asana"
  ],
  "slack": [
    "Slack"
  ],
  "zoom": [
    "Zoom"
  ],
  "twitch": [
    "Twitch"
  ],
  "reddit": [
    "Reddit"
  ],
  "linkedin": [
    "LinkedIn"
  ],
  "pinterest": [
    "Pinterest"
  ],
  "tumblr": [
    "Tumblr"
  ],
  "quora": [
    "Quora"
  ],
  "vk": [
    "VK"
  ],
  "telegram": [
    "Telegram"
  ],
  "signal": [
    "Signal"
  ],
  "viber": [
    "Viber"
  ],
  "ebay": [
    "eBay"
  ],
  "walmart": [
    "Walmart"
  ],
  "target": [
    "Target"
  ],
  "ikea": [
    "IKEA"
  ],
  "etsy": [
    "Etsy"
  ],
  "shein": [
    "Shein"
  ],
  "temu": [
    "Temu"
  ],
  "stripe": [
    "Stripe"
  ],
  "revolut": [
    "Revolut"
  ],
  "wise": [
    "Wise"
  ],
  "klarna": [
    "Klarna"
  ],
  "robinhood": [
    "Robinhood"
  ],
  "kraken": [
    "Kraken"
  ],
  "okx": [
    "OKX"
  ],
  "bbc": [
    "BBC"
  ],
  "cnn": [
    "CNN"
  ],
  "reuters": [
    "Reuters"
  ],
  "bloomberg": [
    "Bloomberg"
  ],
  "forbes": [
    "Forbes"
  ],
  "wired": [
    "Wired"
  ],
  "kayak": [
    "Kayak"
  ],
  "expedia": [
    "Expedia"
  ],
  "grab": [
    "Grab"
  ],
  "didi": [
    "滴滴 DiDi"
  ],
  "strava": [
    "Strava"
  ],
  "fitbit": [
    "Fitbit"
  ],
  "peloton": [
    "Peloton"
  ],
  "calm": [
    "Calm"
  ],
  "headspace": [
    "Headspace"
  ],
  "ea": [
    "EA"
  ],
  "blizzard": [
    "Blizzard"
  ],
  "ubisoft": [
    "Ubisoft"
  ],
  "roblox": [
    "Roblox"
  ],
  "minecraft": [
    "Minecraft"
  ],
  "xbox": [
    "Xbox"
  ],
  "huggingface": [
    "Hugging Face"
  ],
  "runway": [
    "Runway"
  ],
  "suno": [
    "Suno"
  ],
  "pika": [
    "Pika"
  ],
  "elevenlabs": [
    "ElevenLabs"
  ],
  "midjourney": [
    "Midjourney"
  ],
  "perplexity": [
    "Perplexity"
  ],
  "grok": [
    "Grok"
  ],
  "jasper": [
    "Jasper"
  ],
  "cohere": [
    "Cohere"
  ],
  "ideogram": [
    "Ideogram"
  ],
  "luma": [
    "Luma AI"
  ],
  "kling": [
    "可灵 Kling AI"
  ],
  "replicate": [
    "Replicate"
  ],
  "pandora": [
    "Pandora"
  ],
  "deezer": [
    "Deezer"
  ],
  "tidal": [
    "Tidal"
  ],
  "soundcloud": [
    "SoundCloud"
  ],
  "bandcamp": [
    "Bandcamp"
  ],
  "lastfm": [
    "Last.fm"
  ],
  "crunchyroll": [
    "Crunchyroll"
  ],
  "plex": [
    "Plex"
  ],
  "bilibili": [
    "哔哩哔哩 Bilibili"
  ],
  "youku": [
    "优酷 Youku"
  ],
  "iqiyi": [
    "爱奇艺 iQIYI"
  ],
  "hulu": [
    "Hulu"
  ],
  "disney": [
    "Disney+"
  ],
  "paramount": [
    "Paramount+"
  ],
  "peacock": [
    "Peacock"
  ],
  "airtable": [
    "Airtable"
  ],
  "monday": [
    "Monday.com"
  ],
  "clickup": [
    "ClickUp"
  ],
  "miro": [
    "Miro"
  ],
  "linear": [
    "Linear"
  ],
  "obsidian": [
    "Obsidian"
  ],
  "todoist": [
    "Todoist"
  ],
  "loom": [
    "Loom"
  ],
  "zapier": [
    "Zapier"
  ],
  "tripadvisor": [
    "TripAdvisor"
  ],
  "skyscanner": [
    "Skyscanner"
  ],
  "agoda": [
    "Agoda"
  ],
  "ctrip": [
    "携程 Ctrip",
    "Trip.com"
  ],
  "myfitnesspal": [
    "MyFitnessPal"
  ],
  "noom": [
    "Noom"
  ],
  "nike": [
    "Nike Training"
  ],
  "网易": [
    "网易云音乐 NetEase Music",
    "网易邮箱大师",
    "网易游戏",
    "我的世界网易版",
    "蛋仔派对",
    "第五人格",
    "逆水寒手游",
    "梦幻西游",
    "阴阳师",
    "永劫无间",
    "有道云笔记",
    "网易新闻"
  ],
  "netease": [
    "网易云音乐 NetEase Music",
    "网易邮箱大师",
    "网易游戏",
    "我的世界网易版",
    "蛋仔派对",
    "第五人格",
    "逆水寒手游",
    "梦幻西游",
    "阴阳师",
    "永劫无间",
    "有道云笔记",
    "网易新闻"
  ],
  "腾讯": [
    "微信 WeChat",
    "企业微信 WeCom",
    "QQ",
    "QQ邮箱",
    "腾讯会议 VooV Meeting",
    "腾讯视频",
    "腾讯元宝",
    "QQ音乐",
    "酷狗音乐",
    "酷我音乐",
    "全民K歌",
    "王者荣耀",
    "和平精英",
    "腾讯文档",
    "腾讯新闻",
    "微信支付"
  ],
  "字节": [
    "TikTok",
    "抖音 Douyin",
    "西瓜视频",
    "豆包 Doubao",
    "即梦AI",
    "汽水音乐",
    "飞书 Feishu",
    "剪映",
    "今日头条"
  ],
  "字节跳动": [
    "TikTok",
    "抖音 Douyin",
    "西瓜视频",
    "豆包 Doubao",
    "即梦AI",
    "汽水音乐",
    "飞书 Feishu",
    "剪映",
    "今日头条"
  ],
  "百度": [
    "百度贴吧",
    "文心助手",
    "百度网盘",
    "百度地图",
    "爱奇艺 iQIYI"
  ],
  "阿里": [
    "淘宝 Taobao",
    "天猫 Tmall",
    "AliExpress",
    "Lazada",
    "1688",
    "闲鱼",
    "钉钉 DingTalk",
    "千问 Qwen",
    "夸克 Quark",
    "优酷 Youku",
    "阿里云盘",
    "饿了么",
    "飞猪",
    "高德地图",
    "阿里健康"
  ],
  "阿里巴巴": [
    "淘宝 Taobao",
    "天猫 Tmall",
    "AliExpress",
    "Lazada",
    "1688",
    "闲鱼",
    "钉钉 DingTalk",
    "千问 Qwen",
    "夸克 Quark",
    "优酷 Youku",
    "阿里云盘",
    "饿了么",
    "飞猪",
    "高德地图",
    "阿里健康"
  ],
  "alibaba": [
    "淘宝 Taobao",
    "天猫 Tmall",
    "AliExpress",
    "Lazada",
    "1688",
    "闲鱼",
    "钉钉 DingTalk",
    "千问 Qwen",
    "夸克 Quark",
    "优酷 Youku",
    "阿里云盘",
    "饿了么",
    "飞猪",
    "高德地图",
    "阿里健康"
  ],
  "蚂蚁": [
    "支付宝 Alipay"
  ],
  "ant": [
    "支付宝 Alipay"
  ],
  "谷歌": [
    "YouTube",
    "Google Drive",
    "Google Meet",
    "YouTube Music",
    "Gemini",
    "Fitbit"
  ],
  "alphabet": [
    "YouTube",
    "Google Drive",
    "Google Meet",
    "YouTube Music",
    "Gemini",
    "Fitbit"
  ],
  "脸书": [
    "Facebook",
    "Instagram",
    "WhatsApp",
    "Threads",
    "Messenger"
  ],
  "facebook": [
    "Facebook",
    "Instagram",
    "WhatsApp",
    "Threads",
    "Messenger"
  ],
  "微软": [
    "Teams",
    "Copilot",
    "GitHub",
    "Xbox",
    "Minecraft",
    "LinkedIn",
    "Blizzard"
  ],
  "苹果": [
    "Apple Music",
    "Apple TV+"
  ],
  "快手": [
    "快手 Kuaishou",
    "可灵 Kling AI"
  ],
  "kuaishou": [
    "快手 Kuaishou",
    "可灵 Kling AI"
  ],
  "米哈游": [
    "原神 Genshin Impact",
    "崩坏：星穹铁道",
    "绝区零"
  ],
  "mihoyo": [
    "原神 Genshin Impact",
    "崩坏：星穹铁道",
    "绝区零"
  ],
  "hoyoverse": [
    "原神 Genshin Impact",
    "崩坏：星穹铁道",
    "绝区零"
  ],
  "京东": [
    "京东 JD",
    "京东健康"
  ],
  "jd": [
    "京东 JD",
    "京东健康"
  ],
  "jingdong": [
    "京东 JD",
    "京东健康"
  ],
  "拼多多": [
    "拼多多 Pinduoduo",
    "Temu"
  ],
  "pdd": [
    "拼多多 Pinduoduo",
    "Temu"
  ],
  "pinduoduo": [
    "拼多多 Pinduoduo",
    "Temu"
  ],
  "亚马逊": [
    "Amazon",
    "Prime Video",
    "Amazon Music",
    "Twitch"
  ],
  "奥多比": [
    "Adobe Firefly"
  ],
  "adobe": [
    "Adobe Firefly"
  ],
  "stability": [
    "Stable Diffusion"
  ],
  "kakao": [
    "KakaoTalk"
  ],
  "卡카오": [
    "KakaoTalk"
  ],
  "金山": [
    "WPS Office"
  ],
  "kingsoft": [
    "WPS Office"
  ],
  "月之暗面": [
    "Kimi"
  ],
  "moonshot": [
    "Kimi"
  ],
  "智谱": [
    "智谱清言"
  ],
  "zhipu": [
    "智谱清言"
  ],
  "科大讯飞": [
    "讯飞星火"
  ],
  "讯飞": [
    "讯飞星火"
  ],
  "iflytek": [
    "讯飞星火"
  ],
  "携程": [
    "携程 Ctrip",
    "Trip.com"
  ],
  "拳头": [
    "Valorant",
    "League of Legends"
  ],
  "索尼": [
    "PlayStation"
  ],
  "任天堂": [
    "Nintendo"
  ],
  "巨人": [
    "太空杀"
  ],
  "巨人网络": [
    "太空杀"
  ],
  "giant": [
    "太空杀"
  ]
};

type AppEntry = { name: string; cat: string; url: string; color: string; icon: string; initials: string; keywords?: string };

const APPS: AppEntry[] = [
  {"name": "Facebook", "cat": "Social", "url": "https://www.facebook.com", "color": "#1877F2", "icon": "https://www.facebook.com/favicon.ico", "initials": "FB", "keywords": "Facebook"},
  {"name": "Instagram", "cat": "Social", "url": "https://www.instagram.com", "color": "#E1306C", "icon": "https://www.instagram.com/favicon.ico", "initials": "IG", "keywords": "Instagram"},
  {"name": "Twitter / X", "cat": "Social", "url": "https://x.com", "color": "#000000", "icon": "https://abs.twimg.com/favicons/twitter.3.ico", "initials": "X", "keywords": "Twitter / X"},
  {"name": "TikTok", "cat": "Social", "url": "https://www.tiktok.com", "color": "#010101", "icon": "https://www.tiktok.com/favicon.ico", "initials": "TT", "keywords": "TikTok"},
  {"name": "Snapchat", "cat": "Social", "url": "https://www.snapchat.com", "color": "#FFFC00", "icon": "https://www.snapchat.com/favicon.ico", "initials": "SC", "keywords": "Snapchat"},
  {"name": "Pinterest", "cat": "Social", "url": "https://www.pinterest.com", "color": "#E60023", "icon": "https://www.pinterest.com/favicon.ico", "initials": "PT", "keywords": "Pinterest"},
  {"name": "LinkedIn", "cat": "Social", "url": "https://www.linkedin.com", "color": "#0A66C2", "icon": "https://www.linkedin.com/favicon.ico", "initials": "LI", "keywords": "LinkedIn"},
  {"name": "Reddit", "cat": "Social", "url": "https://www.reddit.com", "color": "#FF4500", "icon": "https://www.reddit.com/favicon.ico", "initials": "RD", "keywords": "Reddit"},
  {"name": "微博 Weibo", "cat": "Social", "url": "https://weibo.com", "color": "#E6162D", "icon": "https://weibo.com/favicon.ico", "initials": "WB", "keywords": "Weibo"},
  {"name": "Threads", "cat": "Social", "url": "https://www.threads.com", "color": "#000000", "icon": "https://www.threads.net/favicon.ico", "initials": "TH", "keywords": "Threads"},
  {"name": "Mastodon", "cat": "Social", "url": "https://mastodon.social", "color": "#6364FF", "icon": "https://mastodon.social/favicon.ico", "initials": "MD", "keywords": "Mastodon"},
  {"name": "Tumblr", "cat": "Social", "url": "https://www.tumblr.com", "color": "#35465C", "icon": "https://www.tumblr.com/favicon.ico", "initials": "TB", "keywords": "Tumblr"},
  {"name": "Quora", "cat": "Social", "url": "https://www.quora.com", "color": "#B92B27", "icon": "https://www.quora.com/favicon.ico", "initials": "QR", "keywords": "Quora"},
  {"name": "VK", "cat": "Social", "url": "https://vk.com", "color": "#0077FF", "icon": "https://vk.com/favicon.ico", "initials": "VK", "keywords": "VK"},
  {"name": "Clubhouse", "cat": "Social", "url": "https://www.clubhouse.com", "color": "#F3E8D0", "icon": "https://www.clubhouse.com/favicon.ico", "initials": "CH", "keywords": "Clubhouse"},
  {"name": "BeReal", "cat": "Social", "url": "https://bere.al", "color": "#000000", "icon": "https://bere.al/favicon.ico", "initials": "BR", "keywords": "BeReal"},
  {"name": "小红书 Xiaohongshu", "cat": "Social", "url": "https://www.xiaohongshu.com", "color": "#6366F1", "icon": "https://www.xiaohongshu.com/favicon.ico", "initials": "小红", "keywords": "rednote 小红书"},
  {"name": "知乎 Zhihu", "cat": "Social", "url": "https://www.zhihu.com", "color": "#6366F1", "icon": "https://www.zhihu.com/favicon.ico", "initials": "知乎", "keywords": "知乎"},
  {"name": "百度贴吧", "cat": "Social", "url": "https://tieba.baidu.com", "color": "#6366F1", "icon": "https://tieba.baidu.com/favicon.ico", "initials": "百度", "keywords": "baidu tieba"},
  {"name": "豆瓣 Douban", "cat": "Social", "url": "https://www.douban.com", "color": "#6366F1", "icon": "https://www.douban.com/favicon.ico", "initials": "豆瓣", "keywords": "豆瓣"},
  {"name": "Soul", "cat": "Social", "url": "https://www.soulapp.cn", "color": "#6366F1", "icon": "https://www.soulapp.cn/favicon.ico", "initials": "So", "keywords": "灵魂社交"},
  {"name": "陌陌 Momo", "cat": "Social", "url": "https://www.immomo.com", "color": "#6366F1", "icon": "https://www.immomo.com/favicon.ico", "initials": "陌陌", "keywords": "陌陌"},
  {"name": "Bluesky", "cat": "Social", "url": "https://bsky.app", "color": "#6366F1", "icon": "https://bsky.app/favicon.ico", "initials": "Bl", "keywords": "蓝天"},
  {"name": "微信 WeChat", "cat": "Messaging", "url": "https://www.wechat.com", "color": "#07C160", "icon": "https://www.wechat.com/favicon.ico", "initials": "WC", "keywords": "WeChat"},
  {"name": "WhatsApp", "cat": "Messaging", "url": "https://www.whatsapp.com", "color": "#25D366", "icon": "https://static.whatsapp.net/rsrc.php/v3/yP/r/rYZqPCBaG70.png", "initials": "WA", "keywords": "WhatsApp"},
  {"name": "Telegram", "cat": "Messaging", "url": "https://telegram.org", "color": "#2AABEE", "icon": "https://telegram.org/favicon.ico", "initials": "TG", "keywords": "Telegram"},
  {"name": "Discord", "cat": "Messaging", "url": "https://discord.com", "color": "#5865F2", "icon": "https://discord.com/favicon.ico", "initials": "DC", "keywords": "Discord"},
  {"name": "Slack", "cat": "Messaging", "url": "https://slack.com", "color": "#4A154B", "icon": "https://slack.com/favicon.ico", "initials": "SL", "keywords": "Slack"},
  {"name": "Line", "cat": "Messaging", "url": "https://line.me", "color": "#00B900", "icon": "https://line.me/favicon.ico", "initials": "LN", "keywords": "Line"},
  {"name": "Viber", "cat": "Messaging", "url": "https://www.viber.com", "color": "#7360F2", "icon": "https://www.viber.com/favicon.ico", "initials": "VB", "keywords": "Viber"},
  {"name": "Signal", "cat": "Messaging", "url": "https://signal.org", "color": "#3A76F0", "icon": "https://signal.org/favicon.ico", "initials": "SG", "keywords": "Signal"},
  {"name": "Zoom", "cat": "Messaging", "url": "https://zoom.us", "color": "#2D8CFF", "icon": "https://zoom.us/favicon.ico", "initials": "ZM", "keywords": "Zoom"},
  {"name": "Teams", "cat": "Messaging", "url": "https://teams.microsoft.com", "color": "#6264A7", "icon": "https://teams.microsoft.com/favicon.ico", "initials": "MT", "keywords": "Teams"},
  {"name": "KakaoTalk", "cat": "Messaging", "url": "https://www.kakaocorp.com/page/service/service/KakaoTalk", "color": "#FAE100", "icon": "https://www.kakaocorp.com/favicon.ico", "initials": "KT", "keywords": "KakaoTalk"},
  {"name": "企业微信 WeCom", "cat": "Messaging", "url": "https://work.weixin.qq.com", "color": "#07C160", "icon": "https://work.weixin.qq.com/favicon.ico", "initials": "WW", "keywords": "WeChat Work"},
  {"name": "Google Meet", "cat": "Messaging", "url": "https://meet.google.com", "color": "#00897B", "icon": "https://meet.google.com/favicon.ico", "initials": "GM", "keywords": "Google Meet"},
  {"name": "Webex", "cat": "Messaging", "url": "https://www.webex.com", "color": "#00BCEB", "icon": "https://www.webex.com/favicon.ico", "initials": "WX", "keywords": "Webex"},
  {"name": "QQ", "cat": "Messaging", "url": "https://im.qq.com", "color": "#6366F1", "icon": "https://im.qq.com/favicon.ico", "initials": "QQ", "keywords": "腾讯QQ"},
  {"name": "钉钉 DingTalk", "cat": "Messaging", "url": "https://www.dingtalk.com", "color": "#6366F1", "icon": "https://www.dingtalk.com/favicon.ico", "initials": "钉钉", "keywords": "阿里钉钉"},
  {"name": "飞书 Feishu", "cat": "Messaging", "url": "https://www.feishu.cn", "color": "#6366F1", "icon": "https://www.feishu.cn/favicon.ico", "initials": "飞书", "keywords": "字节飞书"},
  {"name": "腾讯会议 VooV Meeting", "cat": "Messaging", "url": "https://meeting.tencent.com", "color": "#6366F1", "icon": "https://meeting.tencent.com/favicon.ico", "initials": "腾讯", "keywords": "腾讯会议"},
  {"name": "网易邮箱大师", "cat": "Messaging", "url": "https://mail.163.com/dashi/", "color": "#6366F1", "icon": "https://mail.163.com/favicon.ico", "initials": "网易", "keywords": "netease mail 邮箱"},
  {"name": "Messenger", "cat": "Messaging", "url": "https://www.messenger.com", "color": "#6366F1", "icon": "https://www.messenger.com/favicon.ico", "initials": "Me", "keywords": "脸书信使"},
  {"name": "QQ邮箱", "cat": "Messaging", "url": "https://mail.qq.com", "color": "#6366F1", "icon": "https://mail.qq.com/favicon.ico", "initials": "QQ", "keywords": "qq mail 邮箱"},
  {"name": "YouTube", "cat": "Video", "url": "https://www.youtube.com", "color": "#FF0000", "icon": "https://www.youtube.com/favicon.ico", "initials": "YT", "keywords": "YouTube"},
  {"name": "Netflix", "cat": "Video", "url": "https://www.netflix.com", "color": "#E50914", "icon": "https://www.netflix.com/favicon.ico", "initials": "NF", "keywords": "Netflix"},
  {"name": "Twitch", "cat": "Video", "url": "https://www.twitch.tv", "color": "#9146FF", "icon": "https://www.twitch.tv/favicon.ico", "initials": "TW", "keywords": "Twitch"},
  {"name": "Disney+", "cat": "Video", "url": "https://www.disneyplus.com", "color": "#113CCF", "icon": "https://www.disneyplus.com/favicon.ico", "initials": "D+", "keywords": "Disney+"},
  {"name": "Hulu", "cat": "Video", "url": "https://www.hulu.com", "color": "#1CE783", "icon": "https://www.hulu.com/favicon.ico", "initials": "HL", "keywords": "Hulu"},
  {"name": "Prime Video", "cat": "Video", "url": "https://www.primevideo.com", "color": "#00A8E1", "icon": "https://www.primevideo.com/favicon.ico", "initials": "PV", "keywords": "Prime Video"},
  {"name": "哔哩哔哩 Bilibili", "cat": "Video", "url": "https://www.bilibili.com", "color": "#00A1D6", "icon": "https://www.bilibili.com/favicon.ico", "initials": "BL", "keywords": "Bilibili"},
  {"name": "Vimeo", "cat": "Video", "url": "https://vimeo.com", "color": "#1AB7EA", "icon": "https://vimeo.com/favicon.ico", "initials": "VM", "keywords": "Vimeo"},
  {"name": "Dailymotion", "cat": "Video", "url": "https://www.dailymotion.com", "color": "#0066DC", "icon": "https://www.dailymotion.com/favicon.ico", "initials": "DM", "keywords": "Dailymotion"},
  {"name": "Crunchyroll", "cat": "Video", "url": "https://www.crunchyroll.com", "color": "#F47521", "icon": "https://www.crunchyroll.com/favicon.ico", "initials": "CR", "keywords": "Crunchyroll"},
  {"name": "Peacock", "cat": "Video", "url": "https://www.peacocktv.com", "color": "#000000", "icon": "https://www.peacocktv.com/favicon.ico", "initials": "PC", "keywords": "Peacock"},
  {"name": "Paramount+", "cat": "Video", "url": "https://www.paramountplus.com", "color": "#0064FF", "icon": "https://www.paramountplus.com/favicon.ico", "initials": "P+", "keywords": "Paramount+"},
  {"name": "Apple TV+", "cat": "Video", "url": "https://tv.apple.com", "color": "#000000", "icon": "https://tv.apple.com/favicon.ico", "initials": "TV", "keywords": "Apple TV+"},
  {"name": "Plex", "cat": "Video", "url": "https://www.plex.tv", "color": "#E5A00D", "icon": "https://www.plex.tv/favicon.ico", "initials": "PX", "keywords": "Plex"},
  {"name": "爱奇艺 iQIYI", "cat": "Video", "url": "https://www.iqiyi.com", "color": "#00BE06", "icon": "https://www.iqiyi.com/favicon.ico", "initials": "IQ", "keywords": "Iqiyi"},
  {"name": "优酷 Youku", "cat": "Video", "url": "https://www.youku.com", "color": "#1890FF", "icon": "https://www.youku.com/favicon.ico", "initials": "YK", "keywords": "Youku"},
  {"name": "抖音 Douyin", "cat": "Video", "url": "https://www.douyin.com", "color": "#6366F1", "icon": "https://www.douyin.com/favicon.ico", "initials": "抖音", "keywords": "抖音"},
  {"name": "快手 Kuaishou", "cat": "Video", "url": "https://www.kuaishou.com", "color": "#6366F1", "icon": "https://www.kuaishou.com/favicon.ico", "initials": "快手", "keywords": "快手"},
  {"name": "腾讯视频", "cat": "Video", "url": "https://v.qq.com", "color": "#6366F1", "icon": "https://v.qq.com/favicon.ico", "initials": "腾讯", "keywords": "tencent video"},
  {"name": "西瓜视频", "cat": "Video", "url": "https://www.ixigua.com", "color": "#6366F1", "icon": "https://www.ixigua.com/favicon.ico", "initials": "西瓜", "keywords": "xigua video"},
  {"name": "芒果TV", "cat": "Video", "url": "https://www.mgtv.com", "color": "#6366F1", "icon": "https://www.mgtv.com/favicon.ico", "initials": "芒果", "keywords": "mango tv 湖南卫视"},
  {"name": "央视频", "cat": "Video", "url": "https://www.yangshipin.cn", "color": "#6366F1", "icon": "https://www.yangshipin.cn/favicon.ico", "initials": "央视", "keywords": "央视 cctv"},
  {"name": "咪咕视频", "cat": "Video", "url": "https://www.miguvideo.com", "color": "#6366F1", "icon": "https://www.miguvideo.com/favicon.ico", "initials": "咪咕", "keywords": "migu 中国移动"},
  {"name": "Spotify", "cat": "Music", "url": "https://www.spotify.com", "color": "#1DB954", "icon": "https://www.spotify.com/favicon.ico", "initials": "SP", "keywords": "Spotify"},
  {"name": "Apple Music", "cat": "Music", "url": "https://music.apple.com", "color": "#FC3C44", "icon": "https://music.apple.com/favicon.ico", "initials": "AM", "keywords": "Apple Music"},
  {"name": "SoundCloud", "cat": "Music", "url": "https://soundcloud.com", "color": "#FF5500", "icon": "https://soundcloud.com/favicon.ico", "initials": "SC", "keywords": "SoundCloud"},
  {"name": "YouTube Music", "cat": "Music", "url": "https://music.youtube.com", "color": "#FF0000", "icon": "https://music.youtube.com/favicon.ico", "initials": "YM", "keywords": "YouTube Music"},
  {"name": "Deezer", "cat": "Music", "url": "https://www.deezer.com", "color": "#A238FF", "icon": "https://www.deezer.com/favicon.ico", "initials": "DZ", "keywords": "Deezer"},
  {"name": "Tidal", "cat": "Music", "url": "https://tidal.com", "color": "#000000", "icon": "https://tidal.com/favicon.ico", "initials": "TD", "keywords": "Tidal"},
  {"name": "网易云音乐 NetEase Music", "cat": "Music", "url": "https://music.163.com", "color": "#C20C0C", "icon": "https://music.163.com/favicon.ico", "initials": "NM", "keywords": "NetEase Music"},
  {"name": "Pandora", "cat": "Music", "url": "https://www.pandora.com", "color": "#3668FF", "icon": "https://www.pandora.com/favicon.ico", "initials": "PN", "keywords": "Pandora"},
  {"name": "Amazon Music", "cat": "Music", "url": "https://music.amazon.com", "color": "#FF9900", "icon": "https://music.amazon.com/favicon.ico", "initials": "AM", "keywords": "Amazon Music"},
  {"name": "iHeartRadio", "cat": "Music", "url": "https://www.iheart.com", "color": "#C6002B", "icon": "https://www.iheart.com/favicon.ico", "initials": "IH", "keywords": "iHeartRadio"},
  {"name": "Bandcamp", "cat": "Music", "url": "https://bandcamp.com", "color": "#1DA0C3", "icon": "https://bandcamp.com/favicon.ico", "initials": "BC", "keywords": "Bandcamp"},
  {"name": "Last.fm", "cat": "Music", "url": "https://www.last.fm", "color": "#D51007", "icon": "https://www.last.fm/favicon.ico", "initials": "LF", "keywords": "Last.fm"},
  {"name": "QQ音乐", "cat": "Music", "url": "https://y.qq.com", "color": "#6366F1", "icon": "https://y.qq.com/favicon.ico", "initials": "QQ", "keywords": "qq music"},
  {"name": "酷狗音乐", "cat": "Music", "url": "https://www.kugou.com", "color": "#6366F1", "icon": "https://www.kugou.com/favicon.ico", "initials": "酷狗", "keywords": "kugou"},
  {"name": "酷我音乐", "cat": "Music", "url": "https://www.kuwo.cn", "color": "#6366F1", "icon": "https://www.kuwo.cn/favicon.ico", "initials": "酷我", "keywords": "kuwo"},
  {"name": "汽水音乐", "cat": "Music", "url": "https://music.douyin.com/qishui", "color": "#6366F1", "icon": "https://music.douyin.com/favicon.ico", "initials": "汽水", "keywords": "qishui"},
  {"name": "喜马拉雅", "cat": "Music", "url": "https://www.ximalaya.com", "color": "#6366F1", "icon": "https://www.ximalaya.com/favicon.ico", "initials": "喜马", "keywords": "ximalaya 播客 有声书"},
  {"name": "蜻蜓FM", "cat": "Music", "url": "https://www.qtfm.cn", "color": "#6366F1", "icon": "https://www.qtfm.cn/favicon.ico", "initials": "蜻蜓", "keywords": "qingting 播客"},
  {"name": "全民K歌", "cat": "Music", "url": "https://kg.qq.com", "color": "#6366F1", "icon": "https://kg.qq.com/favicon.ico", "initials": "全民", "keywords": "we sing 卡拉ok"},
  {"name": "ChatGPT", "cat": "AI", "url": "https://chatgpt.com", "color": "#10A37F", "icon": "https://chat.openai.com/favicon.ico", "initials": "GP", "keywords": "ChatGPT"},
  {"name": "Claude", "cat": "AI", "url": "https://claude.ai", "color": "#D97757", "icon": "https://claude.ai/favicon.ico", "initials": "CL", "keywords": "Claude"},
  {"name": "Gemini", "cat": "AI", "url": "https://gemini.google.com", "color": "#4285F4", "icon": "https://gemini.google.com/favicon.ico", "initials": "GM", "keywords": "Gemini"},
  {"name": "Midjourney", "cat": "AI", "url": "https://www.midjourney.com", "color": "#000000", "icon": "https://www.midjourney.com/favicon.ico", "initials": "MJ", "keywords": "Midjourney"},
  {"name": "Stable Diffusion", "cat": "AI", "url": "https://stability.ai", "color": "#7C3AED", "icon": "https://stability.ai/favicon.ico", "initials": "SD", "keywords": "Stable Diffusion"},
  {"name": "Perplexity", "cat": "AI", "url": "https://www.perplexity.ai", "color": "#20808D", "icon": "https://www.perplexity.ai/favicon.ico", "initials": "PX", "keywords": "Perplexity"},
  {"name": "Copilot", "cat": "AI", "url": "https://copilot.microsoft.com", "color": "#0078D4", "icon": "https://copilot.microsoft.com/favicon.ico", "initials": "CP", "keywords": "Copilot"},
  {"name": "Grok", "cat": "AI", "url": "https://grok.com", "color": "#000000", "icon": "https://grok.x.ai/favicon.ico", "initials": "GK", "keywords": "Grok"},
  {"name": "Runway", "cat": "AI", "url": "https://runwayml.com", "color": "#000000", "icon": "https://runwayml.com/favicon.ico", "initials": "RW", "keywords": "Runway"},
  {"name": "Suno", "cat": "AI", "url": "https://suno.com", "color": "#7C3AED", "icon": "https://suno.com/favicon.ico", "initials": "SN", "keywords": "Suno"},
  {"name": "Pika", "cat": "AI", "url": "https://pika.art", "color": "#FF6B6B", "icon": "https://pika.art/favicon.ico", "initials": "PK", "keywords": "Pika"},
  {"name": "ElevenLabs", "cat": "AI", "url": "https://elevenlabs.io", "color": "#000000", "icon": "https://elevenlabs.io/favicon.ico", "initials": "EL", "keywords": "ElevenLabs"},
  {"name": "Hugging Face", "cat": "AI", "url": "https://huggingface.co", "color": "#FFD21E", "icon": "https://huggingface.co/favicon.ico", "initials": "HF", "keywords": "Hugging Face"},
  {"name": "Replicate", "cat": "AI", "url": "https://replicate.com", "color": "#000000", "icon": "https://replicate.com/favicon.ico", "initials": "RP", "keywords": "Replicate"},
  {"name": "Jasper", "cat": "AI", "url": "https://www.jasper.ai", "color": "#FF6B35", "icon": "https://www.jasper.ai/favicon.ico", "initials": "JS", "keywords": "Jasper"},
  {"name": "Copy.ai", "cat": "AI", "url": "https://www.copy.ai", "color": "#7C3AED", "icon": "https://www.copy.ai/favicon.ico", "initials": "CA", "keywords": "Copy.ai"},
  {"name": "可灵 Kling AI", "cat": "AI", "url": "https://klingai.com", "color": "#000000", "icon": "https://klingai.com/favicon.ico", "initials": "KL", "keywords": "Kling AI"},
  {"name": "Luma AI", "cat": "AI", "url": "https://lumalabs.ai", "color": "#000000", "icon": "https://lumalabs.ai/favicon.ico", "initials": "LA", "keywords": "Luma AI"},
  {"name": "Ideogram", "cat": "AI", "url": "https://ideogram.ai", "color": "#000000", "icon": "https://ideogram.ai/favicon.ico", "initials": "ID", "keywords": "Ideogram"},
  {"name": "Adobe Firefly", "cat": "AI", "url": "https://firefly.adobe.com", "color": "#FF0000", "icon": "https://firefly.adobe.com/favicon.ico", "initials": "FF", "keywords": "Adobe Firefly"},
  {"name": "Cohere", "cat": "AI", "url": "https://cohere.com", "color": "#39594D", "icon": "https://cohere.com/favicon.ico", "initials": "CO", "keywords": "Cohere"},
  {"name": "豆包 Doubao", "cat": "AI", "url": "https://www.doubao.com", "color": "#6366F1", "icon": "https://www.doubao.com/favicon.ico", "initials": "豆包", "keywords": "豆包"},
  {"name": "DeepSeek 深度求索", "cat": "AI", "url": "https://www.deepseek.com", "color": "#6366F1", "icon": "https://www.deepseek.com/favicon.ico", "initials": "De", "keywords": "deep seek 深度求索"},
  {"name": "千问 Qwen", "cat": "AI", "url": "https://www.qianwen.com", "color": "#6366F1", "icon": "https://www.qianwen.com/favicon.ico", "initials": "千问", "keywords": "通义千问 通义 qianwen tongyi"},
  {"name": "文心助手", "cat": "AI", "url": "https://wenxin.baidu.com", "color": "#6366F1", "icon": "https://wenxin.baidu.com/favicon.ico", "initials": "文心", "keywords": "文小言 文心一言 文言一心 ernie wenxiaoyan"},
  {"name": "腾讯元宝", "cat": "AI", "url": "https://yuanbao.tencent.com", "color": "#6366F1", "icon": "https://yuanbao.tencent.com/favicon.ico", "initials": "腾讯", "keywords": "yuanbao"},
  {"name": "Kimi", "cat": "AI", "url": "https://www.kimi.com", "color": "#6366F1", "icon": "https://www.kimi.com/favicon.ico", "initials": "Ki", "keywords": "月之暗面 moonshot"},
  {"name": "智谱清言", "cat": "AI", "url": "https://chatglm.cn", "color": "#6366F1", "icon": "https://chatglm.cn/favicon.ico", "initials": "智谱", "keywords": "zhipu chatglm glm"},
  {"name": "秘塔AI搜索", "cat": "AI", "url": "https://metaso.cn", "color": "#6366F1", "icon": "https://metaso.cn/favicon.ico", "initials": "秘塔", "keywords": "metaso 秘塔"},
  {"name": "即梦AI", "cat": "AI", "url": "https://jimeng.jianying.com", "color": "#6366F1", "icon": "https://jimeng.jianying.com/favicon.ico", "initials": "即梦", "keywords": "jimeng dreamina"},
  {"name": "讯飞星火", "cat": "AI", "url": "https://xinghuo.xfyun.cn", "color": "#6366F1", "icon": "https://xinghuo.xfyun.cn/favicon.ico", "initials": "讯飞", "keywords": "iflytek spark 科大讯飞"},
  {"name": "Steam", "cat": "Games", "url": "https://store.steampowered.com", "color": "#1B2838", "icon": "https://store.steampowered.com/favicon.ico", "initials": "ST", "keywords": "Steam"},
  {"name": "Epic Games", "cat": "Games", "url": "https://www.epicgames.com", "color": "#313131", "icon": "https://www.epicgames.com/favicon.ico", "initials": "EG", "keywords": "Epic Games"},
  {"name": "Roblox", "cat": "Games", "url": "https://www.roblox.com", "color": "#E02020", "icon": "https://www.roblox.com/favicon.ico", "initials": "RB", "keywords": "Roblox"},
  {"name": "Minecraft", "cat": "Games", "url": "https://www.minecraft.net", "color": "#62B47A", "icon": "https://www.minecraft.net/favicon.ico", "initials": "MC", "keywords": "Minecraft 我的世界国际版"},
  {"name": "Fortnite", "cat": "Games", "url": "https://www.fortnite.com", "color": "#1B1B2F", "icon": "https://www.fortnite.com/favicon.ico", "initials": "FN", "keywords": "Fortnite"},
  {"name": "League of Legends", "cat": "Games", "url": "https://www.leagueoflegends.com", "color": "#C89B3C", "icon": "https://www.leagueoflegends.com/favicon.ico", "initials": "LoL", "keywords": "League of Legends"},
  {"name": "Valorant", "cat": "Games", "url": "https://playvalorant.com", "color": "#FF4655", "icon": "https://playvalorant.com/favicon.ico", "initials": "VL", "keywords": "Valorant"},
  {"name": "PUBG", "cat": "Games", "url": "https://www.pubg.com", "color": "#F5A623", "icon": "https://www.pubg.com/favicon.ico", "initials": "PB", "keywords": "PUBG"},
  {"name": "原神 Genshin Impact", "cat": "Games", "url": "https://genshin.hoyoverse.com", "color": "#4A90D9", "icon": "https://genshin.hoyoverse.com/favicon.ico", "initials": "GI", "keywords": "Genshin Impact"},
  {"name": "Xbox", "cat": "Games", "url": "https://www.xbox.com", "color": "#107C10", "icon": "https://www.xbox.com/favicon.ico", "initials": "XB", "keywords": "Xbox"},
  {"name": "PlayStation", "cat": "Games", "url": "https://www.playstation.com", "color": "#003087", "icon": "https://www.playstation.com/favicon.ico", "initials": "PS", "keywords": "PlayStation"},
  {"name": "Nintendo", "cat": "Games", "url": "https://www.nintendo.com", "color": "#E4000F", "icon": "https://www.nintendo.com/favicon.ico", "initials": "NT", "keywords": "Nintendo"},
  {"name": "GOG", "cat": "Games", "url": "https://www.gog.com", "color": "#86328A", "icon": "https://www.gog.com/favicon.ico", "initials": "GG", "keywords": "GOG"},
  {"name": "Ubisoft", "cat": "Games", "url": "https://www.ubisoft.com", "color": "#0070FF", "icon": "https://www.ubisoft.com/favicon.ico", "initials": "UB", "keywords": "Ubisoft"},
  {"name": "EA", "cat": "Games", "url": "https://www.ea.com", "color": "#FF4747", "icon": "https://www.ea.com/favicon.ico", "initials": "EA", "keywords": "EA"},
  {"name": "Blizzard", "cat": "Games", "url": "https://www.blizzard.com", "color": "#148EFF", "icon": "https://www.blizzard.com/favicon.ico", "initials": "BZ", "keywords": "Blizzard"},
  {"name": "Itch.io", "cat": "Games", "url": "https://itch.io", "color": "#FA5C5C", "icon": "https://itch.io/favicon.ico", "initials": "IT", "keywords": "Itch.io"},
  {"name": "Overwolf", "cat": "Games", "url": "https://www.overwolf.com", "color": "#FF6600", "icon": "https://www.overwolf.com/favicon.ico", "initials": "OW", "keywords": "Overwolf"},
  {"name": "Kongregate", "cat": "Games", "url": "https://www.kongregate.com", "color": "#DF2B2B", "icon": "https://www.kongregate.com/favicon.ico", "initials": "KG", "keywords": "Kongregate"},
  {"name": "我的世界网易版", "cat": "Games", "url": "https://mc.163.com", "color": "#6366F1", "icon": "https://mc.163.com/favicon.ico", "initials": "我的", "keywords": "minecraft china 我的世界中国版"},
  {"name": "太空杀", "cat": "Games", "url": "https://www.spaceparty.cn", "color": "#6366F1", "icon": "https://www.spaceparty.cn/favicon.ico", "initials": "太空", "keywords": "space party 巨人网络"},
  {"name": "网易游戏", "cat": "Games", "url": "https://game.163.com", "color": "#6366F1", "icon": "https://game.163.com/favicon.ico", "initials": "网易", "keywords": "netease games"},
  {"name": "蛋仔派对", "cat": "Games", "url": "https://party.163.com", "color": "#6366F1", "icon": "https://party.163.com/favicon.ico", "initials": "蛋仔", "keywords": "eggy party"},
  {"name": "第五人格", "cat": "Games", "url": "https://id5.163.com", "color": "#6366F1", "icon": "https://id5.163.com/favicon.ico", "initials": "第五", "keywords": "identity v"},
  {"name": "逆水寒手游", "cat": "Games", "url": "https://h.163.com", "color": "#6366F1", "icon": "https://h.163.com/favicon.ico", "initials": "逆水", "keywords": "justice mobile 逆水寒"},
  {"name": "梦幻西游", "cat": "Games", "url": "https://xyq.163.com", "color": "#6366F1", "icon": "https://xyq.163.com/favicon.ico", "initials": "梦幻", "keywords": "fantasy westward journey"},
  {"name": "阴阳师", "cat": "Games", "url": "https://yys.163.com", "color": "#6366F1", "icon": "https://yys.163.com/favicon.ico", "initials": "阴阳", "keywords": "onmyoji"},
  {"name": "永劫无间", "cat": "Games", "url": "https://www.yjwujian.cn", "color": "#6366F1", "icon": "https://www.yjwujian.cn/favicon.ico", "initials": "永劫", "keywords": "naraka bladepoint"},
  {"name": "王者荣耀", "cat": "Games", "url": "https://pvp.qq.com", "color": "#6366F1", "icon": "https://pvp.qq.com/favicon.ico", "initials": "王者", "keywords": "honor of kings"},
  {"name": "和平精英", "cat": "Games", "url": "https://gp.qq.com", "color": "#6366F1", "icon": "https://gp.qq.com/favicon.ico", "initials": "和平", "keywords": "game for peace"},
  {"name": "崩坏：星穹铁道", "cat": "Games", "url": "https://sr.mihoyo.com", "color": "#6366F1", "icon": "https://sr.mihoyo.com/favicon.ico", "initials": "崩坏", "keywords": "honkai star rail 崩坏星穹铁道"},
  {"name": "绝区零", "cat": "Games", "url": "https://zzz.mihoyo.com", "color": "#6366F1", "icon": "https://zzz.mihoyo.com/favicon.ico", "initials": "绝区", "keywords": "zenless zone zero"},
  {"name": "明日方舟", "cat": "Games", "url": "https://ak.hypergryph.com", "color": "#6366F1", "icon": "https://ak.hypergryph.com/favicon.ico", "initials": "明日", "keywords": "arknights 鹰角"},
  {"name": "Amazon", "cat": "Shopping", "url": "https://www.amazon.com", "color": "#FF9900", "icon": "https://www.amazon.com/favicon.ico", "initials": "AZ", "keywords": "Amazon"},
  {"name": "eBay", "cat": "Shopping", "url": "https://www.ebay.com", "color": "#E53238", "icon": "https://www.ebay.com/favicon.ico", "initials": "EB", "keywords": "eBay"},
  {"name": "淘宝 Taobao", "cat": "Shopping", "url": "https://www.taobao.com", "color": "#FF5000", "icon": "https://www.taobao.com/favicon.ico", "initials": "TB", "keywords": "Taobao"},
  {"name": "Shopee", "cat": "Shopping", "url": "https://shopee.com", "color": "#EE4D2D", "icon": "https://shopee.com/favicon.ico", "initials": "SH", "keywords": "Shopee"},
  {"name": "Lazada", "cat": "Shopping", "url": "https://www.lazada.com", "color": "#0F146D", "icon": "https://www.lazada.com/favicon.ico", "initials": "LZ", "keywords": "Lazada"},
  {"name": "Etsy", "cat": "Shopping", "url": "https://www.etsy.com", "color": "#F1641E", "icon": "https://www.etsy.com/favicon.ico", "initials": "ET", "keywords": "Etsy"},
  {"name": "Shein", "cat": "Shopping", "url": "https://www.shein.com", "color": "#000000", "icon": "https://www.shein.com/favicon.ico", "initials": "SN", "keywords": "Shein"},
  {"name": "AliExpress", "cat": "Shopping", "url": "https://www.aliexpress.com", "color": "#FF6A00", "icon": "https://www.aliexpress.com/favicon.ico", "initials": "AE", "keywords": "AliExpress"},
  {"name": "Walmart", "cat": "Shopping", "url": "https://www.walmart.com", "color": "#0071CE", "icon": "https://www.walmart.com/favicon.ico", "initials": "WM", "keywords": "Walmart"},
  {"name": "Target", "cat": "Shopping", "url": "https://www.target.com", "color": "#CC0000", "icon": "https://www.target.com/favicon.ico", "initials": "TG", "keywords": "Target"},
  {"name": "IKEA", "cat": "Shopping", "url": "https://www.ikea.com", "color": "#0058A3", "icon": "https://www.ikea.com/favicon.ico", "initials": "IK", "keywords": "IKEA"},
  {"name": "Zalando", "cat": "Shopping", "url": "https://www.zalando.com", "color": "#FF6900", "icon": "https://www.zalando.com/favicon.ico", "initials": "ZL", "keywords": "Zalando"},
  {"name": "Flipkart", "cat": "Shopping", "url": "https://www.flipkart.com", "color": "#2874F0", "icon": "https://www.flipkart.com/favicon.ico", "initials": "FK", "keywords": "Flipkart"},
  {"name": "Rakuten", "cat": "Shopping", "url": "https://www.rakuten.com", "color": "#BF0000", "icon": "https://www.rakuten.com/favicon.ico", "initials": "RK", "keywords": "Rakuten"},
  {"name": "Wish", "cat": "Shopping", "url": "https://www.wish.com", "color": "#2FB7EC", "icon": "https://www.wish.com/favicon.ico", "initials": "WS", "keywords": "Wish"},
  {"name": "Temu", "cat": "Shopping", "url": "https://www.temu.com", "color": "#FF6600", "icon": "https://www.temu.com/favicon.ico", "initials": "TM", "keywords": "Temu"},
  {"name": "天猫 Tmall", "cat": "Shopping", "url": "https://www.tmall.com", "color": "#6366F1", "icon": "https://www.tmall.com/favicon.ico", "initials": "天猫", "keywords": "天猫"},
  {"name": "京东 JD", "cat": "Shopping", "url": "https://www.jd.com", "color": "#6366F1", "icon": "https://www.jd.com/favicon.ico", "initials": "京东", "keywords": "京东 jingdong"},
  {"name": "拼多多 Pinduoduo", "cat": "Shopping", "url": "https://www.pinduoduo.com", "color": "#6366F1", "icon": "https://www.pinduoduo.com/favicon.ico", "initials": "拼多", "keywords": "拼多多 pdd"},
  {"name": "闲鱼", "cat": "Shopping", "url": "https://www.goofish.com", "color": "#6366F1", "icon": "https://www.goofish.com/favicon.ico", "initials": "闲鱼", "keywords": "xianyu 二手"},
  {"name": "唯品会", "cat": "Shopping", "url": "https://www.vip.com", "color": "#6366F1", "icon": "https://www.vip.com/favicon.ico", "initials": "唯品", "keywords": "vipshop"},
  {"name": "得物", "cat": "Shopping", "url": "https://www.dewu.com", "color": "#6366F1", "icon": "https://www.dewu.com/favicon.ico", "initials": "得物", "keywords": "poizon dewu"},
  {"name": "1688", "cat": "Shopping", "url": "https://www.1688.com", "color": "#6366F1", "icon": "https://www.1688.com/favicon.ico", "initials": "16", "keywords": "阿里巴巴 批发"},
  {"name": "美团", "cat": "Shopping", "url": "https://www.meituan.com", "color": "#6366F1", "icon": "https://www.meituan.com/favicon.ico", "initials": "美团", "keywords": "meituan 外卖"},
  {"name": "饿了么", "cat": "Shopping", "url": "https://www.ele.me", "color": "#6366F1", "icon": "https://www.ele.me/favicon.ico", "initials": "饿了", "keywords": "eleme 外卖"},
  {"name": "PayPal", "cat": "Finance", "url": "https://www.paypal.com", "color": "#003087", "icon": "https://www.paypal.com/favicon.ico", "initials": "PP", "keywords": "PayPal"},
  {"name": "Coinbase", "cat": "Finance", "url": "https://www.coinbase.com", "color": "#0052FF", "icon": "https://www.coinbase.com/favicon.ico", "initials": "CB", "keywords": "Coinbase"},
  {"name": "Binance", "cat": "Finance", "url": "https://www.binance.com", "color": "#F3BA2F", "icon": "https://www.binance.com/favicon.ico", "initials": "BN", "keywords": "Binance"},
  {"name": "Robinhood", "cat": "Finance", "url": "https://robinhood.com", "color": "#00C805", "icon": "https://robinhood.com/favicon.ico", "initials": "RH", "keywords": "Robinhood"},
  {"name": "Stripe", "cat": "Finance", "url": "https://stripe.com", "color": "#635BFF", "icon": "https://stripe.com/favicon.ico", "initials": "SR", "keywords": "Stripe"},
  {"name": "Wise", "cat": "Finance", "url": "https://wise.com", "color": "#00B9FF", "icon": "https://wise.com/favicon.ico", "initials": "WS", "keywords": "Wise"},
  {"name": "Revolut", "cat": "Finance", "url": "https://www.revolut.com", "color": "#0075EB", "icon": "https://www.revolut.com/favicon.ico", "initials": "RV", "keywords": "Revolut"},
  {"name": "Cash App", "cat": "Finance", "url": "https://cash.app", "color": "#00D632", "icon": "https://cash.app/favicon.ico", "initials": "CA", "keywords": "Cash App"},
  {"name": "Venmo", "cat": "Finance", "url": "https://venmo.com", "color": "#3D95CE", "icon": "https://venmo.com/favicon.ico", "initials": "VM", "keywords": "Venmo"},
  {"name": "Kraken", "cat": "Finance", "url": "https://www.kraken.com", "color": "#5741D9", "icon": "https://www.kraken.com/favicon.ico", "initials": "KR", "keywords": "Kraken"},
  {"name": "OKX", "cat": "Finance", "url": "https://www.okx.com", "color": "#000000", "icon": "https://www.okx.com/favicon.ico", "initials": "OK", "keywords": "OKX"},
  {"name": "支付宝 Alipay", "cat": "Finance", "url": "https://www.alipay.com", "color": "#1677FF", "icon": "https://www.alipay.com/favicon.ico", "initials": "AP", "keywords": "Alipay"},
  {"name": "Klarna", "cat": "Finance", "url": "https://www.klarna.com", "color": "#FFB3C7", "icon": "https://www.klarna.com/favicon.ico", "initials": "KL", "keywords": "Klarna"},
  {"name": "微信支付", "cat": "Finance", "url": "https://pay.weixin.qq.com", "color": "#6366F1", "icon": "https://pay.weixin.qq.com/favicon.ico", "initials": "微信", "keywords": "wechat pay weixin pay"},
  {"name": "云闪付", "cat": "Finance", "url": "https://www.95516.com", "color": "#6366F1", "icon": "https://www.95516.com/favicon.ico", "initials": "云闪", "keywords": "unionpay 银联"},
  {"name": "招商银行", "cat": "Finance", "url": "https://www.cmbchina.com", "color": "#6366F1", "icon": "https://www.cmbchina.com/favicon.ico", "initials": "招商", "keywords": "cmb 招行"},
  {"name": "中国工商银行", "cat": "Finance", "url": "https://www.icbc.com.cn", "color": "#6366F1", "icon": "https://www.icbc.com.cn/favicon.ico", "initials": "中国", "keywords": "icbc 工行"},
  {"name": "中国建设银行", "cat": "Finance", "url": "https://www.ccb.com", "color": "#6366F1", "icon": "https://www.ccb.com/favicon.ico", "initials": "中国", "keywords": "ccb 建行"},
  {"name": "中国银行", "cat": "Finance", "url": "https://www.boc.cn", "color": "#6366F1", "icon": "https://www.boc.cn/favicon.ico", "initials": "中国", "keywords": "boc 中行"},
  {"name": "东方财富", "cat": "Finance", "url": "https://www.eastmoney.com", "color": "#6366F1", "icon": "https://www.eastmoney.com/favicon.ico", "initials": "东方", "keywords": "eastmoney"},
  {"name": "同花顺", "cat": "Finance", "url": "https://www.10jqka.com.cn", "color": "#6366F1", "icon": "https://www.10jqka.com.cn/favicon.ico", "initials": "同花", "keywords": "ths 股票"},
  {"name": "Google Drive", "cat": "Productivity", "url": "https://drive.google.com", "color": "#4285F4", "icon": "https://drive.google.com/favicon.ico", "initials": "GD", "keywords": "Google Drive"},
  {"name": "Notion", "cat": "Productivity", "url": "https://www.notion.so", "color": "#000000", "icon": "https://www.notion.so/favicon.ico", "initials": "NO", "keywords": "Notion"},
  {"name": "Trello", "cat": "Productivity", "url": "https://trello.com", "color": "#0052CC", "icon": "https://trello.com/favicon.ico", "initials": "TR", "keywords": "Trello"},
  {"name": "Dropbox", "cat": "Productivity", "url": "https://www.dropbox.com", "color": "#0061FF", "icon": "https://www.dropbox.com/favicon.ico", "initials": "DB", "keywords": "Dropbox"},
  {"name": "Evernote", "cat": "Productivity", "url": "https://evernote.com", "color": "#00A82D", "icon": "https://evernote.com/favicon.ico", "initials": "EN", "keywords": "Evernote"},
  {"name": "Asana", "cat": "Productivity", "url": "https://asana.com", "color": "#F06A6A", "icon": "https://asana.com/favicon.ico", "initials": "AS", "keywords": "Asana"},
  {"name": "Figma", "cat": "Productivity", "url": "https://www.figma.com", "color": "#F24E1E", "icon": "https://www.figma.com/favicon.ico", "initials": "FG", "keywords": "Figma"},
  {"name": "GitHub", "cat": "Productivity", "url": "https://github.com", "color": "#181717", "icon": "https://github.com/favicon.ico", "initials": "GH", "keywords": "GitHub"},
  {"name": "Jira", "cat": "Productivity", "url": "https://www.atlassian.com/software/jira", "color": "#0052CC", "icon": "https://www.atlassian.com/favicon.ico", "initials": "JR", "keywords": "Jira"},
  {"name": "Canva", "cat": "Productivity", "url": "https://www.canva.com", "color": "#00C4CC", "icon": "https://www.canva.com/favicon.ico", "initials": "CV", "keywords": "Canva"},
  {"name": "Airtable", "cat": "Productivity", "url": "https://airtable.com", "color": "#FCB400", "icon": "https://airtable.com/favicon.ico", "initials": "AT", "keywords": "Airtable"},
  {"name": "Monday.com", "cat": "Productivity", "url": "https://monday.com", "color": "#FF3D57", "icon": "https://monday.com/favicon.ico", "initials": "MN", "keywords": "Monday.com"},
  {"name": "ClickUp", "cat": "Productivity", "url": "https://clickup.com", "color": "#7B68EE", "icon": "https://clickup.com/favicon.ico", "initials": "CU", "keywords": "ClickUp"},
  {"name": "Miro", "cat": "Productivity", "url": "https://miro.com", "color": "#FFD02F", "icon": "https://miro.com/favicon.ico", "initials": "MR", "keywords": "Miro"},
  {"name": "Linear", "cat": "Productivity", "url": "https://linear.app", "color": "#5E6AD2", "icon": "https://linear.app/favicon.ico", "initials": "LN", "keywords": "Linear"},
  {"name": "Obsidian", "cat": "Productivity", "url": "https://obsidian.md", "color": "#7C3AED", "icon": "https://obsidian.md/favicon.ico", "initials": "OB", "keywords": "Obsidian"},
  {"name": "Todoist", "cat": "Productivity", "url": "https://todoist.com", "color": "#DB4035", "icon": "https://todoist.com/favicon.ico", "initials": "TD", "keywords": "Todoist"},
  {"name": "Confluence", "cat": "Productivity", "url": "https://www.atlassian.com/software/confluence", "color": "#0052CC", "icon": "https://www.atlassian.com/favicon.ico", "initials": "CF", "keywords": "Confluence"},
  {"name": "Loom", "cat": "Productivity", "url": "https://www.loom.com", "color": "#625DF5", "icon": "https://www.loom.com/favicon.ico", "initials": "LM", "keywords": "Loom"},
  {"name": "Zapier", "cat": "Productivity", "url": "https://zapier.com", "color": "#FF4A00", "icon": "https://zapier.com/favicon.ico", "initials": "ZP", "keywords": "Zapier"},
  {"name": "百度网盘", "cat": "Productivity", "url": "https://pan.baidu.com", "color": "#6366F1", "icon": "https://pan.baidu.com/favicon.ico", "initials": "百度", "keywords": "baidu netdisk 云盘"},
  {"name": "夸克 Quark", "cat": "Productivity", "url": "https://www.quark.cn", "color": "#6366F1", "icon": "https://www.quark.cn/favicon.ico", "initials": "夸克", "keywords": "夸克"},
  {"name": "WPS Office", "cat": "Productivity", "url": "https://www.wps.cn", "color": "#6366F1", "icon": "https://www.wps.cn/favicon.ico", "initials": "WP", "keywords": "金山办公 文档"},
  {"name": "腾讯文档", "cat": "Productivity", "url": "https://docs.qq.com", "color": "#6366F1", "icon": "https://docs.qq.com/favicon.ico", "initials": "腾讯", "keywords": "tencent docs"},
  {"name": "石墨文档", "cat": "Productivity", "url": "https://shimo.im", "color": "#6366F1", "icon": "https://shimo.im/favicon.ico", "initials": "石墨", "keywords": "shimo"},
  {"name": "有道云笔记", "cat": "Productivity", "url": "https://note.youdao.com", "color": "#6366F1", "icon": "https://note.youdao.com/favicon.ico", "initials": "有道", "keywords": "youdao note"},
  {"name": "语雀", "cat": "Productivity", "url": "https://www.yuque.com", "color": "#6366F1", "icon": "https://www.yuque.com/favicon.ico", "initials": "语雀", "keywords": "yuque"},
  {"name": "阿里云盘", "cat": "Productivity", "url": "https://www.alipan.com", "color": "#6366F1", "icon": "https://www.alipan.com/favicon.ico", "initials": "阿里", "keywords": "aliyundrive 阿里网盘"},
  {"name": "剪映", "cat": "Productivity", "url": "https://www.capcut.cn", "color": "#6366F1", "icon": "https://www.capcut.cn/favicon.ico", "initials": "剪映", "keywords": "jianying 视频剪辑"},
  {"name": "BBC", "cat": "News", "url": "https://www.bbc.com", "color": "#BB1919", "icon": "https://www.bbc.com/favicon.ico", "initials": "BBC", "keywords": "BBC"},
  {"name": "CNN", "cat": "News", "url": "https://www.cnn.com", "color": "#CC0000", "icon": "https://www.cnn.com/favicon.ico", "initials": "CNN", "keywords": "CNN"},
  {"name": "Reuters", "cat": "News", "url": "https://www.reuters.com", "color": "#FF8000", "icon": "https://www.reuters.com/favicon.ico", "initials": "RT", "keywords": "Reuters"},
  {"name": "The Guardian", "cat": "News", "url": "https://www.theguardian.com", "color": "#052962", "icon": "https://www.theguardian.com/favicon.ico", "initials": "GD", "keywords": "The Guardian"},
  {"name": "Al Jazeera", "cat": "News", "url": "https://www.aljazeera.com", "color": "#F7A800", "icon": "https://www.aljazeera.com/favicon.ico", "initials": "AJ", "keywords": "Al Jazeera"},
  {"name": "New York Times", "cat": "News", "url": "https://www.nytimes.com", "color": "#000000", "icon": "https://www.nytimes.com/favicon.ico", "initials": "NYT", "keywords": "New York Times"},
  {"name": "Washington Post", "cat": "News", "url": "https://www.washingtonpost.com", "color": "#000000", "icon": "https://www.washingtonpost.com/favicon.ico", "initials": "WP", "keywords": "Washington Post"},
  {"name": "Bloomberg", "cat": "News", "url": "https://www.bloomberg.com", "color": "#000000", "icon": "https://www.bloomberg.com/favicon.ico", "initials": "BL", "keywords": "Bloomberg"},
  {"name": "Forbes", "cat": "News", "url": "https://www.forbes.com", "color": "#000000", "icon": "https://www.forbes.com/favicon.ico", "initials": "FB", "keywords": "Forbes"},
  {"name": "TechCrunch", "cat": "News", "url": "https://techcrunch.com", "color": "#0A8F08", "icon": "https://techcrunch.com/favicon.ico", "initials": "TC", "keywords": "TechCrunch"},
  {"name": "The Verge", "cat": "News", "url": "https://www.theverge.com", "color": "#FA4B2A", "icon": "https://www.theverge.com/favicon.ico", "initials": "TV", "keywords": "The Verge"},
  {"name": "Wired", "cat": "News", "url": "https://www.wired.com", "color": "#000000", "icon": "https://www.wired.com/favicon.ico", "initials": "WR", "keywords": "Wired"},
  {"name": "今日头条", "cat": "News", "url": "https://www.toutiao.com", "color": "#6366F1", "icon": "https://www.toutiao.com/favicon.ico", "initials": "今日", "keywords": "toutiao"},
  {"name": "腾讯新闻", "cat": "News", "url": "https://news.qq.com", "color": "#6366F1", "icon": "https://news.qq.com/favicon.ico", "initials": "腾讯", "keywords": "tencent news"},
  {"name": "网易新闻", "cat": "News", "url": "https://news.163.com", "color": "#6366F1", "icon": "https://news.163.com/favicon.ico", "initials": "网易", "keywords": "netease news"},
  {"name": "新浪新闻", "cat": "News", "url": "https://news.sina.com.cn", "color": "#6366F1", "icon": "https://news.sina.com.cn/favicon.ico", "initials": "新浪", "keywords": "sina news"},
  {"name": "澎湃新闻", "cat": "News", "url": "https://www.thepaper.cn", "color": "#6366F1", "icon": "https://www.thepaper.cn/favicon.ico", "initials": "澎湃", "keywords": "the paper"},
  {"name": "财新", "cat": "News", "url": "https://www.caixin.com", "color": "#6366F1", "icon": "https://www.caixin.com/favicon.ico", "initials": "财新", "keywords": "caixin"},
  {"name": "人民日报", "cat": "News", "url": "https://www.people.com.cn", "color": "#6366F1", "icon": "https://www.people.com.cn/favicon.ico", "initials": "人民", "keywords": "people daily 人民网"},
  {"name": "新华社", "cat": "News", "url": "https://www.news.cn", "color": "#6366F1", "icon": "https://www.news.cn/favicon.ico", "initials": "新华", "keywords": "xinhua 新华网"},
  {"name": "Airbnb", "cat": "Travel", "url": "https://www.airbnb.com", "color": "#FF5A5F", "icon": "https://www.airbnb.com/favicon.ico", "initials": "AB", "keywords": "Airbnb"},
  {"name": "Booking.com", "cat": "Travel", "url": "https://www.booking.com", "color": "#003580", "icon": "https://www.booking.com/favicon.ico", "initials": "BK", "keywords": "Booking.com"},
  {"name": "Expedia", "cat": "Travel", "url": "https://www.expedia.com", "color": "#FFC72C", "icon": "https://www.expedia.com/favicon.ico", "initials": "EX", "keywords": "Expedia"},
  {"name": "TripAdvisor", "cat": "Travel", "url": "https://www.tripadvisor.com", "color": "#34E0A1", "icon": "https://www.tripadvisor.com/favicon.ico", "initials": "TA", "keywords": "TripAdvisor"},
  {"name": "Uber", "cat": "Travel", "url": "https://www.uber.com", "color": "#000000", "icon": "https://www.uber.com/favicon.ico", "initials": "UB", "keywords": "Uber"},
  {"name": "Lyft", "cat": "Travel", "url": "https://www.lyft.com", "color": "#FF00BF", "icon": "https://www.lyft.com/favicon.ico", "initials": "LF", "keywords": "Lyft"},
  {"name": "Kayak", "cat": "Travel", "url": "https://www.kayak.com", "color": "#FF690F", "icon": "https://www.kayak.com/favicon.ico", "initials": "KY", "keywords": "Kayak"},
  {"name": "Skyscanner", "cat": "Travel", "url": "https://www.skyscanner.com", "color": "#0770E3", "icon": "https://www.skyscanner.com/favicon.ico", "initials": "SS", "keywords": "Skyscanner"},
  {"name": "Hotels.com", "cat": "Travel", "url": "https://www.hotels.com", "color": "#D32F2F", "icon": "https://www.hotels.com/favicon.ico", "initials": "HC", "keywords": "Hotels.com"},
  {"name": "Agoda", "cat": "Travel", "url": "https://www.agoda.com", "color": "#5392FF", "icon": "https://www.agoda.com/favicon.ico", "initials": "AG", "keywords": "Agoda"},
  {"name": "Grab", "cat": "Travel", "url": "https://www.grab.com", "color": "#00B14F", "icon": "https://www.grab.com/favicon.ico", "initials": "GB", "keywords": "Grab"},
  {"name": "滴滴 DiDi", "cat": "Travel", "url": "https://www.didiglobal.com", "color": "#FF6600", "icon": "https://www.didiglobal.com/favicon.ico", "initials": "DD", "keywords": "DiDi"},
  {"name": "Trip.com", "cat": "Travel", "url": "https://www.trip.com", "color": "#1890FF", "icon": "https://www.trip.com/favicon.ico", "initials": "CT", "keywords": "Ctrip"},
  {"name": "携程 Ctrip", "cat": "Travel", "url": "https://www.ctrip.com", "color": "#6366F1", "icon": "https://www.ctrip.com/favicon.ico", "initials": "携程", "keywords": "携程旅行"},
  {"name": "去哪儿", "cat": "Travel", "url": "https://www.qunar.com", "color": "#6366F1", "icon": "https://www.qunar.com/favicon.ico", "initials": "去哪", "keywords": "qunar"},
  {"name": "飞猪", "cat": "Travel", "url": "https://www.fliggy.com", "color": "#6366F1", "icon": "https://www.fliggy.com/favicon.ico", "initials": "飞猪", "keywords": "fliggy"},
  {"name": "同程旅行", "cat": "Travel", "url": "https://www.ly.com", "color": "#6366F1", "icon": "https://www.ly.com/favicon.ico", "initials": "同程", "keywords": "tongcheng"},
  {"name": "高德地图", "cat": "Travel", "url": "https://www.amap.com", "color": "#6366F1", "icon": "https://www.amap.com/favicon.ico", "initials": "高德", "keywords": "amap gaode"},
  {"name": "百度地图", "cat": "Travel", "url": "https://map.baidu.com", "color": "#6366F1", "icon": "https://map.baidu.com/favicon.ico", "initials": "百度", "keywords": "baidu maps"},
  {"name": "铁路12306", "cat": "Travel", "url": "https://www.12306.cn", "color": "#6366F1", "icon": "https://www.12306.cn/favicon.ico", "initials": "铁路", "keywords": "火车票 中国铁路"},
  {"name": "马蜂窝", "cat": "Travel", "url": "https://www.mafengwo.cn", "color": "#6366F1", "icon": "https://www.mafengwo.cn/favicon.ico", "initials": "马蜂", "keywords": "mafengwo"},
  {"name": "MyFitnessPal", "cat": "Health", "url": "https://www.myfitnesspal.com", "color": "#0066FF", "icon": "https://www.myfitnesspal.com/favicon.ico", "initials": "MF", "keywords": "MyFitnessPal"},
  {"name": "Headspace", "cat": "Health", "url": "https://www.headspace.com", "color": "#F47D31", "icon": "https://www.headspace.com/favicon.ico", "initials": "HS", "keywords": "Headspace"},
  {"name": "Calm", "cat": "Health", "url": "https://www.calm.com", "color": "#4A90E2", "icon": "https://www.calm.com/favicon.ico", "initials": "CM", "keywords": "Calm"},
  {"name": "Strava", "cat": "Health", "url": "https://www.strava.com", "color": "#FC4C02", "icon": "https://www.strava.com/favicon.ico", "initials": "SV", "keywords": "Strava"},
  {"name": "Fitbit", "cat": "Health", "url": "https://www.fitbit.com", "color": "#00B0B9", "icon": "https://www.fitbit.com/favicon.ico", "initials": "FB", "keywords": "Fitbit"},
  {"name": "Nike Training", "cat": "Health", "url": "https://www.nike.com/ntc-app", "color": "#111111", "icon": "https://www.nike.com/favicon.ico", "initials": "NT", "keywords": "Nike Training"},
  {"name": "Peloton", "cat": "Health", "url": "https://www.onepeloton.com", "color": "#CC0000", "icon": "https://www.onepeloton.com/favicon.ico", "initials": "PL", "keywords": "Peloton"},
  {"name": "Noom", "cat": "Health", "url": "https://www.noom.com", "color": "#4CAF50", "icon": "https://www.noom.com/favicon.ico", "initials": "NM", "keywords": "Noom"},
  {"name": "Keep", "cat": "Health", "url": "https://www.gotokeep.com", "color": "#6366F1", "icon": "https://www.gotokeep.com/favicon.ico", "initials": "Ke", "keywords": "keep 健身"},
  {"name": "薄荷健康", "cat": "Health", "url": "https://www.boohee.com", "color": "#6366F1", "icon": "https://www.boohee.com/favicon.ico", "initials": "薄荷", "keywords": "boohee 饮食"},
  {"name": "丁香医生", "cat": "Health", "url": "https://dxy.com", "color": "#6366F1", "icon": "https://dxy.com/favicon.ico", "initials": "丁香", "keywords": "dingxiang 丁香园"},
  {"name": "京东健康", "cat": "Health", "url": "https://health.jd.com", "color": "#6366F1", "icon": "https://health.jd.com/favicon.ico", "initials": "京东", "keywords": "jd health"},
  {"name": "阿里健康", "cat": "Health", "url": "https://www.alihealth.cn", "color": "#6366F1", "icon": "https://www.alihealth.cn/favicon.ico", "initials": "阿里", "keywords": "alihealth"},
  {"name": "好大夫在线", "cat": "Health", "url": "https://www.haodf.com", "color": "#6366F1", "icon": "https://www.haodf.com/favicon.ico", "initials": "好大", "keywords": "haodafu"},
  {"name": "微医", "cat": "Health", "url": "https://www.guahao.com", "color": "#6366F1", "icon": "https://www.guahao.com/favicon.ico", "initials": "微医", "keywords": "we doctor 挂号"},
  {"name": "平安健康", "cat": "Health", "url": "https://www.jk.cn", "color": "#6366F1", "icon": "https://www.jk.cn/favicon.ico", "initials": "平安", "keywords": "ping an health 平安好医生"},
  {"name": "悦跑圈", "cat": "Health", "url": "https://www.thejoyrun.com", "color": "#6366F1", "icon": "https://www.thejoyrun.com/favicon.ico", "initials": "悦跑", "keywords": "joyrun 跑步"},
  {"name": "咕咚", "cat": "Health", "url": "https://www.codoon.com", "color": "#6366F1", "icon": "https://www.codoon.com/favicon.ico", "initials": "咕咚", "keywords": "codoon 运动"},
];

const THEMES = [
  { name: "Dark", bg: "#0F0F1A", card: "#1A1A2E", header: "#16213E", text: "#E2E8F0", sub: "#94A3B8", border: "#2D3748", badge: "#2D3748", badgeText: "#94A3B8" },
  { name: "Light", bg: "#F1F5F9", card: "#FFFFFF", header: "#FFFFFF", text: "#1E293B", sub: "#64748B", border: "#E2E8F0", badge: "#EEF2FF", badgeText: "#4F46E5" },
  { name: "Ocean", bg: "#0A1628", card: "#0D2137", header: "#0A1628", text: "#E0F2FE", sub: "#7DD3FC", border: "#1E3A5F", badge: "#1E3A5F", badgeText: "#7DD3FC" },
  { name: "Forest", bg: "#0D1F0D", card: "#132613", header: "#0D1F0D", text: "#DCFCE7", sub: "#86EFAC", border: "#1A3A1A", badge: "#1A3A1A", badgeText: "#86EFAC" },
  { name: "Sunset", bg: "#1A0A00", card: "#2D1200", header: "#1A0A00", text: "#FEF3C7", sub: "#FCD34D", border: "#4A2000", badge: "#4A2000", badgeText: "#FCD34D" },
  { name: "Purple", bg: "#0F0A1E", card: "#1A1030", header: "#0F0A1E", text: "#EDE9FE", sub: "#C4B5FD", border: "#2D1F4A", badge: "#2D1F4A", badgeText: "#C4B5FD" },
  { name: "Rose", bg: "#1A0A0F", card: "#2D1020", header: "#1A0A0F", text: "#FFE4E6", sub: "#FDA4AF", border: "#4A1020", badge: "#4A1020", badgeText: "#FDA4AF" },
  { name: "Slate", bg: "#0F172A", card: "#1E293B", header: "#0F172A", text: "#F1F5F9", sub: "#94A3B8", border: "#334155", badge: "#334155", badgeText: "#94A3B8" },
];

function IconBox({ app, theme }: { app: typeof APPS[0]; theme: typeof THEMES[0] }) {
  const [imgError, setImgError] = useState(false);
  const domain = app.url.replace("https://", "").replace("http://", "").split("/")[0];
  const googleFavicon = `https://www.google.com/s2/favicons?domain=${domain}&sz=64`;

  if (!imgError) {
    return (
      <img
        src={googleFavicon}
        alt={app.name}
        className="w-8 h-8 rounded-lg object-contain"
        onError={() => setImgError(true)}
      />
    );
  }
  return (
    <div
      className="w-8 h-8 rounded-lg flex items-center justify-center text-white text-xs font-bold flex-shrink-0"
      style={{ backgroundColor: app.color }}
    >
      {app.initials}
    </div>
  );
}

// Normalize full-width text, case and spacing for Chinese/English search.
const normalizeSearch = (value: string) => value.normalize("NFKC").toLowerCase().replace(/\s+/g, "").trim();
const findBrandKeys = (query: string) => {
  const exact = Object.keys(BRAND_ALIASES).filter(key => normalizeSearch(key) === query);
  if (exact.length) return exact;
  return query.length >= 2 ? Object.keys(BRAND_ALIASES).filter(key => normalizeSearch(key).startsWith(query)) : [];
};

function App() {
  const [lang, setLang] = useState("zh");
  const [themeIdx, setThemeIdx] = useState(0);
  const [search, setSearch] = useState("");
  const [activeCat, setActiveCat] = useState("All");
  const [copiedUrl, setCopiedUrl] = useState<string | null>(null);
  const [showLangMenu, setShowLangMenu] = useState(false);
  const [page, setPage] = useState<"home" | "about">("home");

  const t = LANGUAGES[lang];
  const theme = THEMES[themeIdx];

  const filtered = useMemo(() => {
    const q = normalizeSearch(search);
    if (!q) return APPS.filter(app => activeCat === "All" || app.cat === activeCat);

    // Check if query matches a brand alias
    const brandKeys = findBrandKeys(q);
    const brandNames = new Set(brandKeys.flatMap(key => BRAND_ALIASES[key]));

    return APPS.filter(app => {
      const matchCat = activeCat === "All" || app.cat === activeCat;
      const matchSearch =
        normalizeSearch(app.name).includes(q) ||
        normalizeSearch(app.url).includes(q) ||
        normalizeSearch(app.keywords || "").includes(q) ||
        brandNames.has(app.name);
      return matchCat && matchSearch;
    });
  }, [search, activeCat]);

  const brandMatchLabel = useMemo(() => {
    const q = normalizeSearch(search);
    if (!q) return null;
    const brandKey = findBrandKeys(q)[0];
    if (!brandKey) return null;
    const brandNames = BRAND_ALIASES[brandKey];
    const hasExtra = filtered.some(a => brandNames.includes(a.name));
    return hasExtra ? brandKey.charAt(0).toUpperCase() + brandKey.slice(1) : null;
  }, [search, filtered]);

  const grouped = useMemo(() => {
    if (!filtered.length) return {};
    if (activeCat !== "All") return { [activeCat]: filtered };
    const g: Record<string, typeof APPS> = {};
    filtered.forEach(app => {
      if (!g[app.cat]) g[app.cat] = [];
      g[app.cat].push(app);
    });
    return g;
  }, [filtered, activeCat]);

  const handleCopy = (url: string) => {
    navigator.clipboard.writeText(url).then(() => {
      setCopiedUrl(url);
      setTimeout(() => setCopiedUrl(null), 2000);
    });
  };

  const catCounts: Record<string, number> = useMemo(() => {
    const c: Record<string, number> = { All: APPS.length };
    APPS.forEach(a => { c[a.cat] = (c[a.cat] || 0) + 1; });
    return c;
  }, []);

  return (
    <div
      className="min-h-screen top-clearance bottom-clearance"
      style={{ backgroundColor: theme.bg, color: theme.text, fontFamily: "'Segoe UI', system-ui, sans-serif" }}
      onClick={() => showLangMenu && setShowLangMenu(false)}
    >
      {/* Header */}
      <div style={{ backgroundColor: theme.header, borderBottom: `1px solid ${theme.border}` }} className="sticky top-0 z-30 px-4 py-3">
        <div className="max-w-7xl mx-auto">
          {/* Title centered */}
          <div className="text-center mb-3">
            <h1
              className="text-3xl sm:text-5xl font-black tracking-tight"
              style={{
                fontFamily: "'Georgia', 'Times New Roman', serif",
                color: theme.text,
                letterSpacing: "-0.03em",
                textShadow: themeIdx === 0 ? "0 0 40px rgba(139,92,246,0.5)" : "none"
              }}
            >
              {t.title}
            </h1>
            <p className="text-xs sm:text-sm mt-1" style={{ color: theme.sub }}>{t.subtitle}</p>
          </div>
          <div className="flex items-center justify-between mb-3">
            <div />
            <div className="flex items-center gap-2">
              {/* Theme switcher */}
              <div className="flex gap-1 flex-wrap justify-end max-w-[120px] sm:max-w-none">
                {THEMES.map((th, i) => (
                  <button
                    key={th.name}
                    onClick={() => setThemeIdx(i)}
                    title={th.name}
                    className="w-5 h-5 rounded-full border-2 transition-transform hover:scale-110"
                    style={{
                      backgroundColor: th.card,
                      borderColor: i === themeIdx ? theme.text : "transparent",
                      outline: i === themeIdx ? `2px solid ${theme.text}` : "none",
                      outlineOffset: "1px"
                    }}
                  />
                ))}
              </div>
              {/* About button */}
              <button
                onClick={() => setPage(page === "about" ? "home" : "about")}
                className="flex items-center gap-1 px-2 py-1.5 rounded-lg text-xs font-medium border transition-colors"
                style={{
                  borderColor: page === "about" ? "#818CF8" : theme.border,
                  backgroundColor: page === "about" ? "#818CF8" : theme.card,
                  color: page === "about" ? "#fff" : theme.text
                }}
              >
                <svg className="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                <span>{t.about}</span>
              </button>
              {/* Language switcher */}
              <div className="relative">
                <button
                  onClick={(e) => { e.stopPropagation(); setShowLangMenu(!showLangMenu); }}
                  className="flex items-center gap-1 px-2 py-1.5 rounded-lg text-xs font-medium border transition-colors"
                  style={{ borderColor: theme.border, backgroundColor: theme.card, color: theme.text }}
                >
                  <svg className="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129" /></svg>
                  <span className="hidden sm:inline">{LANGUAGES[lang].name}</span>
                  <span className="sm:hidden">{lang.toUpperCase()}</span>
                </button>
                {showLangMenu && (
                  <div
                    className="absolute right-0 top-full mt-1 rounded-xl shadow-2xl z-50 overflow-y-auto"
                    style={{ backgroundColor: theme.card, border: `1px solid ${theme.border}`, maxHeight: "300px", width: "160px" }}
                    onClick={e => e.stopPropagation()}
                  >
                    {Object.entries(LANGUAGES).map(([code, l]) => (
                      <button
                        key={code}
                        onClick={() => { setLang(code); setShowLangMenu(false); }}
                        className="w-full text-left px-3 py-2 text-xs hover:opacity-80 transition-opacity"
                        style={{
                          color: code === lang ? "#818CF8" : theme.text,
                          fontWeight: code === lang ? 700 : 400,
                          backgroundColor: code === lang ? theme.badge : "transparent"
                        }}
                      >
                        {l.name}
                      </button>
                    ))}
                  </div>
                )}
              </div>
            </div>
          </div>
          {/* Search */}
          <div className="relative">
            <svg className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4" style={{ color: theme.sub }} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
            <input
              type="text"
              value={search}
              onChange={e => setSearch(e.target.value)}
              placeholder={t.search}
              className="w-full pl-9 pr-4 py-2 rounded-xl text-sm outline-none border transition-colors"
              style={{ backgroundColor: theme.card, borderColor: theme.border, color: theme.text }}
            />
          </div>
          {/* Category tabs */}
          <div className="flex gap-1.5 mt-3 overflow-x-auto pb-1 scrollbar-hide">
            {["All", ...CATEGORIES].map(cat => (
              <button
                key={cat}
                onClick={() => setActiveCat(cat === "All" ? "All" : cat)}
                className="flex-shrink-0 px-3 py-1 rounded-full text-xs font-semibold transition-all"
                style={{
                  backgroundColor: activeCat === cat || (cat === "All" && activeCat === "All") ? "#818CF8" : theme.badge,
                  color: activeCat === cat || (cat === "All" && activeCat === "All") ? "#fff" : theme.badgeText,
                }}
              >
                {cat === "All" ? t.allCats : (t.cats[cat] || cat)} {catCounts[cat] ? `(${catCounts[cat]})` : ""}
              </button>
            ))}
          </div>
        </div>
      </div>

      {/* Content */}
      <div className="max-w-7xl mx-auto px-4 py-6">
        {page === "about" ? (
          <div className="max-w-3xl mx-auto">
            {/* About Hero */}
            <div className="rounded-2xl p-6 sm:p-10 mb-6 border text-center" style={{ backgroundColor: theme.card, borderColor: theme.border }}>
              <div className="w-16 h-16 rounded-2xl mx-auto mb-4 flex items-center justify-center" style={{ backgroundColor: "#818CF8" }}>
                <svg className="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" /></svg>
              </div>
              <h2 className="text-2xl sm:text-3xl font-black mb-3" style={{ fontFamily: "'Georgia', serif", color: theme.text }}>{t.aboutTitle}</h2>
              <p className="text-sm sm:text-base leading-relaxed" style={{ color: theme.sub }}>{t.aboutDesc}</p>
            </div>

            {/* Features Grid */}
            <div className="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
              {[
                { icon: "M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z", title: t.feat1Title, desc: t.feat1Desc, color: "#10B981" },
                { icon: "M4 6h16M4 10h16M4 14h16M4 18h16", title: t.feat2Title, desc: t.feat2Desc, color: "#818CF8" },
                { icon: "M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129", title: t.feat3Title, desc: t.feat3Desc, color: "#F59E0B" },
                { icon: "M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01", title: t.feat4Title, desc: t.feat4Desc, color: "#EC4899" },
                { icon: "M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z", title: t.feat5Title, desc: t.feat5Desc, color: "#06B6D4" },
              ].map((f, i) => (
                <div key={i} className="rounded-xl p-4 border flex gap-3" style={{ backgroundColor: theme.card, borderColor: theme.border }}>
                  <div className="w-9 h-9 rounded-lg flex-shrink-0 flex items-center justify-center" style={{ backgroundColor: f.color + "22" }}>
                    <svg className="w-5 h-5" style={{ color: f.color }} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d={f.icon} /></svg>
                  </div>
                  <div>
                    <p className="text-sm font-bold mb-1" style={{ color: theme.text }}>{f.title}</p>
                    <p className="text-xs leading-relaxed" style={{ color: theme.sub }}>{f.desc}</p>
                  </div>
                </div>
              ))}
            </div>

            {/* Usage scenarios */}
            <div className="rounded-2xl p-5 border mb-6" style={{ backgroundColor: theme.card, borderColor: theme.border }}>
              <h3 className="text-base font-bold mb-3" style={{ fontFamily: "'Georgia', serif", color: theme.text }}>{t.usageTitle}</h3>
              <ul className="space-y-2">
                {[t.usage1, t.usage2, t.usage3, t.usage4].map((u, i) => (
                  <li key={i} className="flex items-start gap-2 text-sm" style={{ color: theme.sub }}>
                    <span className="w-5 h-5 rounded-full flex-shrink-0 flex items-center justify-center text-xs font-bold text-white mt-0.5" style={{ backgroundColor: "#818CF8" }}>{i + 1}</span>
                    {u}
                  </li>
                ))}
              </ul>
            </div>

            {/* Stats */}
            <div className="grid grid-cols-3 gap-3 mb-6">
              {[
                { value: `${APPS.length}`, label: "Apps" },
                { value: `${Object.keys(LANGUAGES).length}`, label: t.label },
                { value: `${THEMES.length}`, label: "Themes" },
              ].map((s, i) => (
                <div key={i} className="rounded-xl p-4 border text-center" style={{ backgroundColor: theme.card, borderColor: theme.border }}>
                  <p className="text-2xl font-black" style={{ color: "#818CF8", fontFamily: "'Georgia', serif" }}>{s.value}</p>
                  <p className="text-xs mt-1" style={{ color: theme.sub }}>{s.label}</p>
                </div>
              ))}
            </div>

            <div className="text-center">
              <button
                onClick={() => setPage("home")}
                className="px-6 py-2.5 rounded-xl text-sm font-semibold text-white transition-all hover:opacity-90 active:scale-95"
                style={{ backgroundColor: "#818CF8" }}
              >
                {t.nav} →
              </button>
            </div>
          </div>
        ) : (
          <>
        {/* Brand match banner */}
        {brandMatchLabel && (
          <div className="flex items-center gap-2 mb-4 px-3 py-2 rounded-xl border" style={{ backgroundColor: theme.card, borderColor: "#818CF8" }}>
            <div className="w-6 h-6 rounded-lg flex items-center justify-center flex-shrink-0" style={{ backgroundColor: "#818CF8" }}>
              <svg className="w-3.5 h-3.5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4" /></svg>
            </div>
            <p className="text-xs font-semibold" style={{ color: "#818CF8" }}>
              {t.brandResults}: <span className="font-black">{brandMatchLabel}</span>
            </p>
          </div>
        )}
        {Object.keys(grouped).length === 0 ? (
          <div className="text-center py-24" style={{ color: theme.sub }}>
            <div className="w-16 h-16 mx-auto mb-4 rounded-2xl flex items-center justify-center" style={{ backgroundColor: theme.card, border: `1px solid ${theme.border}` }}>
              <svg className="w-8 h-8 opacity-40" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={1.5} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
            </div>
            <p className="text-base font-semibold mb-1" style={{ color: theme.text }}>
              {t.noResults}
            </p>
            <p className="text-sm" style={{ color: theme.sub }}>{t.noResultsSub} <span className="font-semibold" style={{ color: "#818CF8" }}>"{search}"</span></p>
            <button
              onClick={() => { setSearch(""); setActiveCat("All"); }}
              className="mt-4 px-4 py-2 rounded-xl text-xs font-semibold text-white transition-all hover:opacity-90"
              style={{ backgroundColor: "#818CF8" }}
            >
              {t.clearSearch}
            </button>
          </div>
        ) : (
          Object.entries(grouped).map(([cat, apps]) => (
            <div key={cat} className="mb-8">
              <div className="flex items-center gap-2 mb-3">
                <h2
                  className="text-base sm:text-lg font-bold"
                  style={{ fontFamily: "'Georgia', serif", color: theme.text }}
                >
                  {t.cats[cat] || cat}
                </h2>
                <span className="text-xs px-2 py-0.5 rounded-full" style={{ backgroundColor: theme.badge, color: theme.badgeText }}>
                  {apps.length}
                </span>
              </div>
              <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-3">
                {apps.map(app => (
                  <div
                    key={app.url}
                    className="rounded-xl p-3 flex items-center gap-3 border transition-all hover:scale-[1.02]"
                    style={{ backgroundColor: theme.card, borderColor: theme.border }}
                  >
                    <IconBox app={app} theme={theme} />
                    <div className="flex-1 min-w-0">
                      <p className="text-sm font-semibold truncate" style={{ color: theme.text }}>{app.name}</p>
                      <p className="text-xs truncate" style={{ color: theme.sub }}>{app.url.replace("https://", "")}</p>
                    </div>
                    <button
                      onClick={() => handleCopy(app.url)}
                      className="flex-shrink-0 px-2 py-1.5 rounded-lg text-xs font-semibold transition-all active:scale-95"
                      style={{
                        backgroundColor: copiedUrl === app.url ? "#10B981" : "#818CF8",
                        color: "#fff",
                        minWidth: "60px",
                        textAlign: "center"
                      }}
                    >
                      {copiedUrl === app.url ? t.copied : t.copy}
                    </button>
                  </div>
                ))}
              </div>
            </div>
          ))
        )}
          </>
        )}
      </div>

      {/* Footer */}
      <div className="text-center py-4 text-xs" style={{ color: theme.sub, borderTop: `1px solid ${theme.border}` }}>
        {APPS.length} apps · {Object.keys(LANGUAGES).length} languages · {THEMES.length} themes
      </div>
    </div>
  );
}

createRoot(document.getElementById("root")!).render(<App />);
