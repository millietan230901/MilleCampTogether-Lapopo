# 旅遊網頁範本使用方式

之後提供新的住宿地點、日期、參與人員，以及想保留的頁籤，我會以 `trip-config.js` 作為旅程基本資料來源，並同步更新對應頁面的住宿資訊、行程與內容。

## 基本資料

修改 `trip-config.js` 的基本欄位：

| 欄位 | 用途 |
| --- | --- |
| `id` | 旅程唯一代碼；新旅程請更換，避免清單或分帳資料混用 |
| `pageTitle`、`title`、`tagline` | 瀏覽器標題、主標題與副標 |
| `dateText`、`scheduleDayLabels` | 日期區間與行程日標籤 |
| `location`、`venue`、`room`、`participants` | 地址、住宿名稱、房型、參加人員 |
| `families` | 分帳頁預設的家庭/成員 |
| `checkInName`、`checkInInfo` | 入園報名字與入園、停車、寄放行李、入住等備註 |
| `mapUrl`、`mapImage`、`mapAreaLabel`、`mapTitle`、`mapDescription` | 導航與園區地圖資訊 |
| `venueWebsiteUrl`、`roomInfoUrl`、`venueWebsiteLabel`、`roomInfoLabel` | 住宿官網及房型連結 |
| `tabs`、`tabLabels`、`menuTabTitle` | 要顯示的頁籤、順序及頁籤名稱 |
| `images` | 頁首、資訊圖卡、營區、規定、行程、餐點等圖片 |

## 住宿資訊與行前提醒

- `venueHighlightsTitle`、`venueHighlights`：概覽中的營區設施/服務卡片，例如停車、行李寄放、入住時間；每筆可設 `title`、`detail`、`icon`。
- `breakfast`：早餐 `time`、`title`、`detail`。帶有 `key: "breakfast"` 的設施卡、行程事件或晚餐分類會使用這份設定，避免早餐時間在多處不一致。
- `venueGalleryTitle`、`publicFacilities`、`venueFacilities`：營區實景/公共設備，以及帳篷或房型內設備。圖片分別放在 `images.venueGallery`；設備文字放在對應陣列，每筆可設 `title`、`detail`、`icon`。
- `accommodationTitle`、`accommodationNote`：住宿設備區標題與補充說明。
- `rulesTitle`、`rulesNoticeLabel`、`accommodationRules`、`rulesImagesTitle`：住宿須知與重要守則；規定圖片放在 `images.rules`。規定請依該住宿的最新資訊更新。
- `remindersTitle`、`reminders`：行前提醒/自備用品清單。

沒有提供的設施、設備或規定可留空陣列；空白區會隱藏，不會沿用其他住宿的內容。

## 行程

`scheduleTitle`、`scheduleReferencesTitle` 和 `scheduleDays` 控制行程。每天可設定 `label`、`title`、`references`、`events`：

```js
{
    label: "11/14(六)",
    title: "入住日",
    references: [{
        title: "住宿地點",
        url: "https://example.com/info",
        mapUrl: "https://www.google.com/maps/search/?api=1&query=住宿地點"
    }],
    events: [{
        time: "15:00",
        title: "辦理入住",
        detail: "入住備註",
        mapUrl: "https://www.google.com/maps/search/?api=1&query=住宿地點",
        parking: {
            name: "住宿停車場",
            mapUrl: "https://www.google.com/maps/search/?api=1&query=住宿停車場"
        },
        mealType: "lunch",
        backupRestaurants: [
            { name: "", mapUrl: "" },
            { name: "", mapUrl: "" },
            { name: "", mapUrl: "" },
            { name: "", mapUrl: "" },
            { name: "", mapUrl: "" }
        ]
    }]
}
```

`references` 與 `events` 都可選填 `mapUrl`，設定後會在行程卡右側顯示小型 Google 導航鍵；可用 `mapLabel` 自訂文字。景點可設定 `parking: { name, mapUrl, mapLabel }`，在事件說明下方顯示停車位置與專屬導航鍵。午餐/晚餐事件可加 `mealType: "lunch"` 或 `"dinner"` 及 `backupRestaurants` 陣列；備案餐廳以可收合區塊呈現，最多可填 5 間，每筆使用 `{ name, mapUrl, mapLabel }`。只有填入餐廳名稱或地圖網址的項目會顯示；全部未填時整個備案餐廳區塊隱藏。

請在需要導航的商店、住宿、餐廳、景點和停車場項目設定網址。可使用 Google 地圖分享連結，或用 `https://www.google.com/maps/search/?api=1&query=地點名稱` 建立地點搜尋連結。

事件可用 `key: "breakfast"` 套用 `breakfast` 設定。若要在個別行程卡片顯示照片，可設定 `imageUrl`（可選 `imageAlt`）；圖片固定顯示為 300×150 px，並以裁切方式維持一致版面。使用相對於網頁的路徑，例如 `image/day1_3.jpg`，並將照片放入對應資料夾。不要使用 `/Images/...` 這種從網站根目錄起算且大小寫不符的路徑。整體行程圖片也可放在 `images.schedule`；行程中的餐廳/餐點圖片放在 `images.scheduleMeals`。

## 營區晚餐

`menuTabTitle` 控制頁籤名稱（目前為「營區晚餐」）；`dinnerLabel`、`dinnerTitle`、`dinnerSubtitle` 控制晚餐標題與說明；`dinnerCategories` 是餐點分類和品項，每筆可設定 `title`、`icon`、`color`、`items`。`familyAssignments` 用於各家分工，每筆格式為 `{ family: "家庭名稱", items: ["準備項目"] }`；沒有分工時可留空。`dinnerReminder` 是晚餐下方的補充提醒。晚餐圖片放在 `images.dinner` 或 `images.menuGallery`。

## 個人用品與食材/裝備清單

`checklists` 可為頁籤預先提供預設項目；空陣列代表尚未設定預設清單，使用者仍可在網頁新增項目。

- `personal` 項目：`{ id, category, name, qty, checked }`
- `food` 項目：`{ id, section, category, name, qty, plate, note, checked }`
- `gear` 項目：`{ id, section, category, name, qty, note, checked }`

同一清單中的 `id` 請保持唯一。勾選、編輯與清單同步功能會沿用網頁既有的操作方式。

`checklistCategories.personal`、`.food`、`.gear` 設定新增項目的分類選單；每筆可用 `{ value, label }`，食材/裝備分類也可加上 Tailwind 色彩名稱 `color`。`checklistGroups.food` 和 `.gear` 設定分區標題與說明，每筆用 `{ id, title, detail }`。分區 `id` 是網頁內部代碼，請沿用範本的 `cold`、`ambient`、`cooking`、`kitchen`，只修改顯示文字。`checklistGroups.shoppingTitle`、`shoppingDescription` 可調整採買區標題與說明。

## 圖片替換

所有行程圖片都在 `trip-config.js` 的 `images` 區塊設定，不需要改 HTML。圖片檔放在 `image` 資料夾，`src` 使用相對於網頁的路徑，例如 `image/new-map.jpg`。你可以直接提供圖檔並告訴我用途，我會放入資料夾並更新對應欄位。

- `hero`：頁首背景，沒有設定就不顯示背景圖。
- `mapImage`：住宿地圖。也可改用 `images.map`，格式與下方單張圖片相同。
- `dinner`：菜色/餐點主圖，沒有設定就隱藏。
- `overview`、`venueGallery`、`rules`、`schedule`、`scheduleMeals`、`menuGallery`：可放多張圖的區域；空陣列會自動隱藏。

單張圖片可用路徑字串，也可提供替代文字與圖說：

```js
dinner: {
    src: "image/dinner.jpg",
    alt: "晚餐照片",
    caption: "露營晚餐"
}
```

多張圖片使用物件陣列：

```js
venueGallery: [
    { src: "image/campsite.jpg", alt: "營區草地", caption: "帳篷區" },
    { src: "image/cabin.jpg", alt: "住宿帳篷", caption: "豪華神殿帳" }
]
```

`id` 是這趟旅程的唯一識別碼。每次建立新旅程時都要換成新的英文/數字/連字號 ID，避免不同旅程的清單和分帳資料互相混用。

## 頁籤

在 `tabs` 陣列中依照想要的順序填入下列代碼：

| 代碼 | 頁籤 |
| --- | --- |
| `info` | 行前概覽 |
| `schedule` | 行程安排 |
| `menu` | 營區晚餐 |
| `personal` | 個人用品 |
| `checklist` | 食材裝備 |
| `split` | 分帳工具 |

可以在 `tabLabels` 調整頁籤名稱。沒有列在 `tabs` 的頁籤會隱藏。

## 需要提供的資料

可以直接告訴我新的住宿名稱/地址、入住日期、房型、參加的大人/小孩或家庭名單，以及要保留的頁籤；若要更新行程或餐食，也請一併提供相關內容。住宿設施、交通方式、規定、行程和餐食會依新行程資料更新，不會只改頁首而保留不適用的舊資訊。提供圖片時請說明用途（例如地圖、頁首、營區實景、規定、行程或菜單）；我會把圖檔放進 `image` 資料夾並設定對應圖片欄位。
