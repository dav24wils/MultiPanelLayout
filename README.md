# MultiPanelLayout

A flexible, lightweight `ViewGroup` that adds dockable bottom panels to any Android layout.  
Drag the handles to expand or collapse panels with smooth, snap‑driven animations.

No Jetpack. No external libraries. Just a single, pure‑Android class that you drop into your project.

---

## ✨ Features

- 📐 **Tabbed or stacked docks** – panels can sit side‑by‑side as tabs (single 48 dp row) or stacked vertically.
- 🎚️ **Smooth drag‑to‑expand** – pull a handle up to reveal the panel; animations snap to collapsed, half‑open, or fully expanded states.
- 🏞️ **Landscape‑aware** – optionally hide the dock when the device rotates to landscape, keeping the top view full‑screen.
- 🔒 **Lock in landscape** – prevent panels from opening when in landscape mode.
- 🎛️ **Panel state callbacks** – receive `onPanelExpanded` and `onPanelCollapsed` events.
- ⚙️ **Fill mode** – make the expanded panel fill the entire usable height (via `setFillMode(true)`).
- 🧩 **Zero dependencies** – built directly on `ViewGroup` and the Android SDK. Works on API 18+.

---

## 📦 Installation

### Option 1 – JitPack (recommended)

Add the JitPack repository to your root `build.gradle` (or `settings.gradle`):

```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Then add the dependency:

```gradle
dependencies {
    implementation 'com.github.dav24wils:MultiPanelLayout:Tag'
}
```

Replace Tag with the latest release tag (e.g., v1.0.0).

Option 2 – Copy the source file

MultiPanelLayout is a single Java file. Copy it into your project – no additional configuration required.

---

🚀 Quick Start

1. Add MultiPanelLayout to your layout

```xml
<com.ikechi.studio.MultiPanelLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/multi_panel"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:dockStyle="tabbed"
    app:lockInLandscape="true"
    app:hideDockedInLandscape="true">

    <!-- child[0] → TOP VIEW (main content) -->
    <FrameLayout
        android:id="@+id/top_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#000000" />

    <!-- child[1] → PANEL 0 – Audio library -->
    <LinearLayout
        android:id="@+id/panel_audio"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <!-- Handle – first child, 48dp tall -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:gravity="center"
            android:background="@drawable/handle_audio">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="AUDIO"
                android:textColor="#FFFFFF"
                android:textStyle="bold"
                android:textSize="11sp" />
        </LinearLayout>

        <!-- Panel body – your content here -->
        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/audioList"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:background="#0D0D1A" />
    </LinearLayout>

    <!-- child[2] → PANEL 1 – Video library (same structure) -->
    <LinearLayout ... />

</com.ikechi.studio.MultiPanelLayout>
```

2. Set a state listener (optional)

```java
MultiPanelLayout multiPanel = findViewById(R.id.multi_panel);
multiPanel.setOnPanelStateChangeListener(new MultiPanelLayout.OnPanelStateChangeListener() {
    @Override public void onPanelExpanded(int panelIndex) {
        // panel at index `panelIndex` is now fully expanded
    }
    @Override public void onPanelCollapsed(int panelIndex) {
        // panel closed
    }
});
```

3. Control panels programmatically

```java
// Expand a panel by index (0‑based, among the panels – not counting the top view)
multiPanel.expandPanel(0);

// Collapse whatever panel is currently expanded
multiPanel.collapseAll();

// Check if a panel is fully expanded
if (multiPanel.isPanelExpanded(0)) { ... }

// Toggle fill mode (expanded panel covers the entire usable height)
multiPanel.setFillMode(true);
```

---

⚙️ XML Attributes

Attribute Type Default Description
app:dockStyle enum stacked stacked – panels sit on top of each other; tabbed – a single row of tabs.
app:lockInLandscape boolean false If true, panels cannot be expanded when the device is in landscape orientation.
app:hideDockedInLandscape boolean true If true and lockInLandscape is also true, the dock is completely hidden in landscape.

---

🧱 Layout Requirements

1. The first child of MultiPanelLayout is the top view – it always occupies the upper portion of the screen. Its height is resized as panels expand.
2. All remaining children are panels. Their order in the XML determines their panel index (0, 1, 2, …).
3. Each panel’s first child must be a handle view with a fixed height of 48 dp. The handle is what the user drags to expand/collapse the panel.
4. Panel bodies (the content that becomes visible when expanded) must follow the handle inside the panel’s layout. Use android:layout_weight="1" to let them fill the remaining space.

---

📐 How It Works

· Stacked mode – every panel handle is visible at the bottom, stacked vertically. The total dock height equals numberOfPanels × 48 dp + (numberOfPanels‑1) × gap.
· Tabbed mode – all panel handles share a single row. When collapsed, each handle gets an equal fraction of the row width; when a panel is expanded, the body fills the entire screen width.
· Animation – drag gestures are velocity‑tracked. On release, the panel snaps to either collapsed, half‑open, or fully expanded based on the drag distance and velocity.

---

📄 License

```
MIT License

Copyright (c) 2025 David Wilson Okere

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

👤 Author

David Wilson Okere

· GitHub: dav24wils
· Repository: MultiPanelLayout

---

⭐ Show your support

If this project helped you, please consider giving it a ⭐ on GitHub – it really helps!
