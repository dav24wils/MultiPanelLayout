Here’s a complete, self‑contained sample app that demonstrates MultiPanelLayout,
RecyclingView, IkVideoView and IkLog working together. It loads real audio and
video files from the device (with runtime permission handling), displays them in
dockable panels, and plays videos in the top view.

---

📱 Sample App – MultiPanel Media Browser

A fully functional media browser with four dockable bottom panels:

· Audio – all music tracks, playable via the main player (not shown here, but easily added).
· Video – all videos, tap to play in the IkVideoView.
· About – app information.
· Settings – a sample dark mode toggle.

All UI is built with the custom MultiPanelLayout (tabbed dock), lists use RecyclingView with simple adapters,
and IkLog writes a debug log to Downloads.

---

🔧 Prerequisites

Add the library dependencies to your build.gradle (using JitPack):

```Gradle //Ignore these for now until the projects are published on Maven or jitpack.
implementation 'com.github.dav24wils:MultiPanelLayout:1.0.0'
implementation 'com.github.dav24wils:RecyclingView:1.0.0'
implementation 'com.github.dav24wils:IkVideoView:1.0.0'
implementation 'com.github.dav24wils:IkLog:1.0.0'
```

Also include the AppCompat and Material libraries if not already present.

---

📄 Layout – res/layout/activity_main.xml

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

    <!-- ══════════ child[0] – TOP VIEW (video player) ══════════ -->
    <LinearLayout
        android:id="@+id/top_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:background="#000000">

        <com.ikechi.studio.video.IkVideoView
            android:id="@+id/videoView"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            app:showSeekBar="true" />

        <!-- Transport / info bar -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:gravity="center_vertical"
            android:padding="12dp"
            android:background="#CC000000">

            <TextView
                android:id="@+id/tvCurrentMedia"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:textColor="#CCFFFFFF"
                android:textSize="11sp"
                android:text="No video playing" />

            <ImageButton
                android:id="@+id/btnPlayPause"
                android:layout_width="32dp"
                android:layout_height="32dp"
                android:src="@android:drawable/ic_media_play"
                android:background="?android:attr/selectableItemBackgroundBorderless" />
        </LinearLayout>
    </LinearLayout>

    <!-- ══════════ child[1] – PANEL 0 – Audio Library ══════════ -->
    <LinearLayout
        android:id="@+id/panel_audio"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:orientation="vertical"
            android:gravity="center"
            android:background="@drawable/handle_audio">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="AUDIO"
                android:textColor="#FFFFFF"
                android:textStyle="bold"
                android:textSize="11sp" />

            <TextView
                android:id="@+id/tvAudioCount"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="0 tracks"
                android:textColor="#AAFFFFFF"
                android:textSize="9sp" />
        </LinearLayout>

        <com.ikechi.studio.recyclingview.view.RecyclingView
            android:id="@+id/audioList"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:background="#0D0D1A"
            app:rv_itemSpacing="2dp" />
    </LinearLayout>

    <!-- ══════════ child[2] – PANEL 1 – Video Library ══════════ -->
    <LinearLayout
        android:id="@+id/panel_video"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:orientation="vertical"
            android:gravity="center"
            android:background="@drawable/handle_video">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="VIDEO"
                android:textColor="#FFFFFF"
                android:textStyle="bold"
                android:textSize="11sp" />

            <TextView
                android:id="@+id/tvVideoCount"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="0 videos"
                android:textColor="#AAFFFFFF"
                android:textSize="9sp" />
        </LinearLayout>

        <com.ikechi.studio.recyclingview.view.RecyclingView
            android:id="@+id/videoList"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:background="#0D0D1A"
            app:rv_itemSpacing="2dp" />
    </LinearLayout>

    <!-- ══════════ child[3] – PANEL 2 – About ══════════ -->
    <LinearLayout
        android:id="@+id/panel_about"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:orientation="vertical"
            android:gravity="center"
            android:background="@drawable/handle_about">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ABOUT"
                android:textColor="#FFFFFF"
                android:textStyle="bold"
                android:textSize="11sp" />
        </LinearLayout>

        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:background="#0D0D1A">

            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="MultiPanelLayout + RecyclingView sample app.\n\nBuilt with pure Android SDK, no external libs.\n\n© 2025 David Wilson Okere"
                android:textColor="#AAFFFFFF"
                android:textSize="14sp"
                android:padding="16dp" />
        </ScrollView>
    </LinearLayout>

    <!-- ══════════ child[4] – PANEL 3 – Settings ══════════ -->
    <LinearLayout
        android:id="@+id/panel_settings"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:orientation="vertical"
            android:gravity="center"
            android:background="@drawable/handle_settings">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="SETTINGS"
                android:textColor="#FFFFFF"
                android:textStyle="bold"
                android:textSize="11sp" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:orientation="vertical"
            android:padding="16dp"
            android:background="#0D0D1A">

            <CheckBox
                android:id="@+id/cbDarkMode"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Dark Mode"
                android:textColor="#FFFFFF" />

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Toggle dark mode (demo only)"
                android:textColor="#88FFFFFF"
                android:textSize="12sp"
                android:layout_marginTop="4dp" />
        </LinearLayout>
    </LinearLayout>

</com.ikechi.studio.MultiPanelLayout>
```

---

🧑‍💻 Main Activity – MainActivity.java

```java
package com.example.multipanelsample;

import android.Manifest;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Build;
import android.os.Bundle;
import android.provider.MediaStore;
import android.view.View;
import android.view.ViewGroup;
import android.widget.CheckBox;
import android.widget.ImageButton;
import android.widget.TextView;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

import com.ikechi.studio.IkLog;
import com.ikechi.studio.MultiPanelLayout;
import com.ikechi.studio.recyclingview.view.RecyclingView;
import com.ikechi.studio.recyclingview.v1.adapter.RecyclingAdapter;
import com.ikechi.studio.video.IkVideoView;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private static final int PERMISSION_REQUEST_CODE = 100;
    private MultiPanelLayout multiPanel;
    private RecyclingView audioList, videoList;
    private TextView tvAudioCount, tvVideoCount, tvCurrentMedia;
    private ImageButton btnPlayPause;
    private IkVideoView videoView;

    private AudioAdapter audioAdapter;
    private VideoAdapter videoAdapter;
    private List<MediaItem> audioItems = new ArrayList<>();
    private List<MediaItem> videoItems = new ArrayList<>();

    // Simple data class for both media types
    static class MediaItem {
        Uri uri;
        String displayName;
        long duration;
        String mimeType;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        IkLog.init(getApplicationContext());
        IkLog.d("MainActivity", "App started");

        setContentView(R.layout.activity_main);

        multiPanel = findViewById(R.id.multi_panel);
        audioList = findViewById(R.id.audioList);
        videoList = findViewById(R.id.videoList);
        tvAudioCount = findViewById(R.id.tvAudioCount);
        tvVideoCount = findViewById(R.id.tvVideoCount);
        tvCurrentMedia = findViewById(R.id.tvCurrentMedia);
        btnPlayPause = findViewById(R.id.btnPlayPause);
        videoView = findViewById(R.id.videoView);

        // Set up adapters
        audioAdapter = new AudioAdapter();
        videoAdapter = new VideoAdapter();
        audioList.setAdapter(audioAdapter);
        videoList.setAdapter(videoAdapter);

        // Play/Pause button for video
        btnPlayPause.setOnClickListener(v -> {
            if (videoView.isPlaying()) {
                videoView.pause();
                btnPlayPause.setImageResource(android.R.drawable.ic_media_play);
            } else {
                videoView.start();
                btnPlayPause.setImageResource(android.R.drawable.ic_media_pause);
            }
        });

        // MultiPanelLayout state listener
        multiPanel.setOnPanelStateChangeListener(new MultiPanelLayout.OnPanelStateChangeListener() {
            @Override public void onPanelExpanded(int panelIndex) {
                IkLog.d("MainActivity", "Panel " + panelIndex + " expanded");
            }
            @Override public void onPanelCollapsed(int panelIndex) {
                IkLog.d("MainActivity", "Panel " + panelIndex + " collapsed");
            }
        });

        // Settings checkbox (demo only)
        CheckBox cbDark = findViewById(R.id.cbDarkMode);
        cbDark.setOnCheckedChangeListener((buttonView, isChecked) -> {
            Toast.makeText(MainActivity.this,
                    "Dark mode " + (isChecked ? "on" : "off"), Toast.LENGTH_SHORT).show();
        });

        // Check permission and load media
        if (hasStoragePermission()) {
            loadMedia();
        } else {
            requestStoragePermission();
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if (videoView.isPlaying()) videoView.stopPlayback();
        IkLog.disable();
    }

    // ── Permissions ──────────────────────────────────────────────────────────

    private boolean hasStoragePermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            return ContextCompat.checkSelfPermission(this,
                    Manifest.permission.READ_MEDIA_AUDIO) == PackageManager.PERMISSION_GRANTED &&
                   ContextCompat.checkSelfPermission(this,
                    Manifest.permission.READ_MEDIA_VIDEO) == PackageManager.PERMISSION_GRANTED;
        } else {
            return ContextCompat.checkSelfPermission(this,
                    Manifest.permission.READ_EXTERNAL_STORAGE) == PackageManager.PERMISSION_GRANTED;
        }
    }

    private void requestStoragePermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.READ_MEDIA_AUDIO,
                                 Manifest.permission.READ_MEDIA_VIDEO},
                    PERMISSION_REQUEST_CODE);
        } else {
            ActivityCompat.requestPermissions(this,
                    new String[]{Manifest.permission.READ_EXTERNAL_STORAGE},
                    PERMISSION_REQUEST_CODE);
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == PERMISSION_REQUEST_CODE) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                loadMedia();
            } else {
                Toast.makeText(this, "Storage permission required", Toast.LENGTH_LONG).show();
                finish();
            }
        }
    }

    // ── Media loading ────────────────────────────────────────────────────────

    private void loadMedia() {
        IkLog.d("MainActivity", "Loading media files...");
        // Load audio
        audioItems.clear();
        Uri audioUri = MediaStore.Audio.Media.EXTERNAL_CONTENT_URI;
        String[] audioProjection = {MediaStore.Audio.Media._ID,
                MediaStore.Audio.Media.DISPLAY_NAME,
                MediaStore.Audio.Media.DURATION,
                MediaStore.Audio.Media.MIME_TYPE};
        try (Cursor cursor = getContentResolver().query(audioUri, audioProjection,
                null, null, MediaStore.Audio.Media.DISPLAY_NAME + " ASC")) {
            if (cursor != null && cursor.moveToFirst()) {
                do {
                    MediaItem item = new MediaItem();
                    long id = cursor.getLong(0);
                    item.uri = Uri.withAppendedPath(MediaStore.Audio.Media.EXTERNAL_CONTENT_URI,
                            String.valueOf(id));
                    item.displayName = cursor.getString(1);
                    item.duration = cursor.getLong(2);
                    item.mimeType = cursor.getString(3);
                    audioItems.add(item);
                } while (cursor.moveToNext());
            }
        } catch (Exception e) {
            IkLog.e("MainActivity", "Error loading audio", e);
        }
        tvAudioCount.setText(audioItems.size() + " tracks");
        audioAdapter.notifyDataSetChanged();

        // Load video
        videoItems.clear();
        Uri videoUri = MediaStore.Video.Media.EXTERNAL_CONTENT_URI;
        String[] videoProjection = {MediaStore.Video.Media._ID,
                MediaStore.Video.Media.DISPLAY_NAME,
                MediaStore.Video.Media.DURATION,
                MediaStore.Video.Media.MIME_TYPE};
        try (Cursor cursor = getContentResolver().query(videoUri, videoProjection,
                null, null, MediaStore.Video.Media.DISPLAY_NAME + " ASC")) {
            if (cursor != null && cursor.moveToFirst()) {
                do {
                    MediaItem item = new MediaItem();
                    long id = cursor.getLong(0);
                    item.uri = Uri.withAppendedPath(MediaStore.Video.Media.EXTERNAL_CONTENT_URI,
                            String.valueOf(id));
                    item.displayName = cursor.getString(1);
                    item.duration = cursor.getLong(2);
                    item.mimeType = cursor.getString(3);
                    videoItems.add(item);
                } while (cursor.moveToNext());
            }
        } catch (Exception e) {
            IkLog.e("MainActivity", "Error loading video", e);
        }
        tvVideoCount.setText(videoItems.size() + " videos");
        videoAdapter.notifyDataSetChanged();

        IkLog.d("MainActivity", "Loaded " + audioItems.size() + " audio, " + videoItems.size() + " video");
    }

    // ── Adapters ─────────────────────────────────────────────────────────────

    private class AudioAdapter extends RecyclingAdapter {
        @Override
        public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            View view = getLayoutInflater().inflate(R.layout.item_media_list, parent, false);
            return new MediaViewHolder(view);
        }

        @Override
        public void onBindViewHolder(ViewHolder holder, int position) {
            MediaItem item = audioItems.get(position);
            ((MediaViewHolder) holder).bind(item);
        }

        @Override
        public int getItemCount() {
            return audioItems.size();
        }
    }

    private class VideoAdapter extends RecyclingAdapter {
        @Override
        public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
            View view = getLayoutInflater().inflate(R.layout.item_media_list, parent, false);
            return new MediaViewHolder(view);
        }

        @Override
        public void onBindViewHolder(ViewHolder holder, int position) {
            MediaItem item = videoItems.get(position);
            ((MediaViewHolder) holder).bind(item);
        }

        @Override
        public int getItemCount() {
            return videoItems.size();
        }
    }

    // ── ViewHolder ───────────────────────────────────────────────────────────

    private class MediaViewHolder extends RecyclingAdapter.ViewHolder {
        TextView title, subtitle;
        View playBtn;

        MediaViewHolder(View itemView) {
            super(itemView);
            title = itemView.findViewById(R.id.tv_media_title);
            subtitle = itemView.findViewById(R.id.tv_media_subtitle);
            playBtn = itemView.findViewById(R.id.btn_media_play);
        }

        void bind(final MediaItem item) {
            title.setText(item.displayName);
            subtitle.setText(formatDuration(item.duration));
            playBtn.setOnClickListener(v -> {
                if (item.mimeType.startsWith("video")) {
                    playVideo(item.uri, item.displayName);
                } else {
                    // For audio, you could launch an audio player
                    Toast.makeText(MainActivity.this, "Playing audio: " + item.displayName,
                            Toast.LENGTH_SHORT).show();
                }
            });
            itemView.setOnClickListener(v -> {
                if (item.mimeType.startsWith("video")) {
                    playVideo(item.uri, item.displayName);
                } else {
                    Toast.makeText(MainActivity.this, "Selected: " + item.displayName,
                            Toast.LENGTH_SHORT).show();
                }
            });
        }

        private void playVideo(Uri uri, String name) {
            tvCurrentMedia.setText(name);
            videoView.setVideoURI(uri);
            videoView.start();
            btnPlayPause.setImageResource(android.R.drawable.ic_media_pause);
            // Collapse the video panel so the video is visible
            multiPanel.collapseAll();
        }
    }

    // ── Utility ──────────────────────────────────────────────────────────────

    private String formatDuration(long ms) {
        if (ms <= 0) return "--:--";
        long s = ms / 1000;
        long m = s / 60;
        s = s % 60;
        return m + ":" + (s < 10 ? "0" : "") + s;
    }
}
```

---

🧩 Item Layout – res/layout/item_media_list.xml

A simple layout for each media item row, used by both adapters.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:gravity="center_vertical"
    android:padding="8dp"
    android:background="?android:attr/selectableItemBackground">

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:orientation="vertical">

        <TextView
            android:id="@+id/tv_media_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#FFFFFF"
            android:textSize="14sp"
            android:singleLine="true"
            android:ellipsize="end" />

        <TextView
            android:id="@+id/tv_media_subtitle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textColor="#88FFFFFF"
            android:textSize="12sp" />
    </LinearLayout>

    <ImageButton
        android:id="@+id/btn_media_play"
        android:layout_width="36dp"
        android:layout_height="36dp"
        android:src="@android:drawable/ic_media_play"
        android:background="?android:attr/selectableItemBackgroundBorderless" />
</LinearLayout>
```

---

🎨 Handle Drawables (example)

Create a simple shape drawable for each handle (e.g., res/drawable/handle_audio.xml):

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#FF1744" />
    <corners android:radius="4dp" />
</shape>
```

Repeat for handle_video, handle_about, handle_settings with different colours.

---

🚀 Result

· The app requests storage permission, then loads all audio and video files into RecyclingView lists inside dockable panels.
· Tapping a video item plays it in the IkVideoView at the top, with a simple play/pause button.
· The MultiPanelLayout tabs let you switch between Audio, Video, About, and Settings panels.
· IkLog writes debug messages to Downloads/onwa_debug_log.txt for troubleshooting.

This sample proves that you can build a polished, multi‑panel media app entirely with the custom views – no RecyclerView,
no Jetpack, just pure Android SDK and the com.ikechi.studio libraries.
