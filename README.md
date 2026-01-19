<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />
</div>

# Robotics Spatial Understanding / ロボット空間認識

An interactive demo application showcasing how Google's Gemini AI provides robots with critical spatial understanding capabilities through advanced computer vision.

インタラクティブなデモアプリケーション。Google Gemini AIがロボットに高度なコンピュータビジョンを通じて重要な空間認識機能を提供する様子を紹介します。

View your app in AI Studio: https://ai.studio/apps/drive/1DQHJ-JXd7fBEjbh1EAmshh_jFUn1irbg

---

## 📋 目次 / Table of Contents

- [概要 / Overview](#-概要--overview)
- [主な機能 / Features](#-主な機能--features)
- [アーキテクチャ / Architecture](#-アーキテクチャ--architecture)
- [データフロー / Data Flow](#-データフロー--data-flow)
- [コンポーネント構成 / Component Structure](#-コンポーネント構成--component-structure)
- [技術スタック / Technology Stack](#-技術スタック--technology-stack)
- [セットアップ / Setup](#-セットアップ--setup)
- [使い方 / Usage](#-使い方--usage)

---

## 🌟 概要 / Overview

This application demonstrates the power of Gemini AI in understanding spatial relationships and objects within images. It provides five distinct detection modes that can be used for robotics applications, inventory management, accessibility features, and more.

このアプリケーションは、画像内の空間的関係やオブジェクトを理解するGemini AIの力を実証します。ロボティクスアプリケーション、在庫管理、アクセシビリティ機能などに使用できる5つの異なる検出モードを提供します。

---

## ✨ 主な機能 / Features

### 1. 2D Bounding Boxes / 2D境界ボックス
Detect and label objects with rectangular bounding boxes.
オブジェクトを矩形の境界ボックスで検出およびラベル付けします。

### 2. Segmentation Masks / セグメンテーションマスク
Perform pixel-level segmentation of objects in images.
画像内のオブジェクトをピクセルレベルでセグメント化します。

### 3. Points / ポイント
Identify specific points of interest within images.
画像内の特定の関心点を識別します。

### 4. Price Prediction / 価格予測
Detect items and estimate their market prices in Japanese Yen (JPY).
アイテムを検出し、日本円（JPY）での市場価格を推定します。

### 5. Text Extraction / テキスト抽出
Extract and transcribe all visible text from images.
画像から表示されているすべてのテキストを抽出および転写します。

---

## 🏗️ アーキテクチャ / Architecture

```mermaid
graph TB
    subgraph "Frontend Application / フロントエンドアプリケーション"
        UI[React UI Components<br/>Reactコンポーネント]
        State[Jotai State Management<br/>状態管理]
        Canvas[Canvas Drawing<br/>キャンバス描画]
    end
    
    subgraph "Image Processing / 画像処理"
        Upload[Image Upload/Selection<br/>画像アップロード/選択]
        Resize[Image Resizing<br/>画像リサイズ]
        Overlay[Drawing Overlay<br/>描画オーバーレイ]
    end
    
    subgraph "AI Processing / AI処理"
        API[Google Gemini API<br/>Gemini API]
        Models[AI Models<br/>gemini-robotics-er-1.5-preview<br/>gemini-2.5-flash]
    end
    
    subgraph "Output / 出力"
        Visualization[Visual Overlay<br/>視覚的オーバーレイ]
        JSON[JSON Response<br/>JSONレスポンス]
    end
    
    UI --> State
    State --> Upload
    Upload --> Resize
    Resize --> Overlay
    Overlay --> API
    API --> Models
    Models --> JSON
    JSON --> Visualization
    Visualization --> UI
    Canvas --> Overlay
    
    style UI fill:#3B68FF,color:#fff
    style API fill:#34A853,color:#fff
    style Models fill:#FBBC04,color:#000
    style Visualization fill:#EA4335,color:#fff
```

---

## 🔄 データフロー / Data Flow

```mermaid
sequenceDiagram
    participant User as ユーザー<br/>User
    participant UI as UIコンポーネント<br/>UI Components
    participant State as 状態管理<br/>State (Jotai)
    participant Canvas as キャンバス<br/>Canvas
    participant API as Gemini API
    participant Display as 表示<br/>Display

    User->>UI: 画像選択/アップロード<br/>Select/Upload Image
    UI->>State: 画像ソース保存<br/>Store Image Source
    State->>Display: 画像表示<br/>Display Image
    
    User->>UI: 検出タイプ選択<br/>Select Detection Type
    UI->>State: 検出タイプ更新<br/>Update Detection Type
    
    opt ユーザーが描画する場合<br/>User Draws
        User->>Canvas: 描画<br/>Draw
        Canvas->>State: 描画データ保存<br/>Store Drawing
    end
    
    User->>UI: プロンプト入力<br/>Enter Prompt
    User->>UI: 送信クリック<br/>Click Send
    
    UI->>Canvas: 画像リサイズ (640px max)<br/>Resize Image
    Canvas->>Canvas: 描画を合成<br/>Composite Drawings
    
    UI->>API: API リクエスト送信<br/>Send API Request
    Note over API: 画像 + プロンプト + 設定<br/>Image + Prompt + Config
    
    API-->>UI: JSONレスポンス<br/>JSON Response
    UI->>State: 検出結果保存<br/>Store Detection Results
    State->>Display: 結果可視化<br/>Visualize Results
    
    Display-->>User: 境界ボックス/マスク/ポイント表示<br/>Show Boxes/Masks/Points
```

---

## 🧩 コンポーネント構成 / Component Structure

```mermaid
graph TD
    App[App.tsx<br/>メインアプリケーション]
    
    App --> TopBar[TopBar.tsx<br/>トップバー]
    App --> Content[Content.tsx<br/>画像表示エリア]
    App --> JsonDisplay[JsonDisplay<br/>API表示]
    App --> ExtraModeControls[ExtraModeControls.tsx<br/>追加コントロール]
    App --> ExampleImages[ExampleImages.tsx<br/>サンプル画像]
    App --> SideControls[SideControls.tsx<br/>サイドコントロール]
    App --> DetectTypeSelector[DetectTypeSelector.tsx<br/>検出タイプ選択]
    App --> Prompt[Prompt.tsx<br/>プロンプト入力]
    
    Content --> Canvas[Canvas Drawing<br/>キャンバス描画]
    Content --> BoundingBoxes[Bounding Boxes<br/>境界ボックス]
    Content --> Masks[Segmentation Masks<br/>セグメンテーションマスク]
    Content --> Points[Point Markers<br/>ポイントマーカー]
    
    Prompt --> API[Gemini API Integration<br/>Gemini API統合]
    
    State[atoms.tsx<br/>Jotai状態管理] -.-> App
    State -.-> Content
    State -.-> Prompt
    State -.-> DetectTypeSelector
    
    Types[Types.tsx<br/>型定義] -.-> App
    Utils[utils.tsx<br/>ユーティリティ] -.-> Content
    Utils -.-> Prompt
    Consts[consts.tsx<br/>定数] -.-> Content
    
    style App fill:#3B68FF,color:#fff
    style Content fill:#34A853,color:#fff
    style Prompt fill:#FBBC04,color:#000
    style State fill:#EA4335,color:#fff
```

### 主要コンポーネントの説明 / Key Component Descriptions

#### App.tsx
メインアプリケーションコンポーネント。レイアウトと主要コンポーネントを統合します。
Main application component that integrates layout and primary components.

#### Content.tsx
画像表示とインタラクティブな描画機能を提供します。検出結果をオーバーレイ表示します。
Provides image display and interactive drawing functionality. Overlays detection results.

#### Prompt.tsx
ユーザー入力、モデル選択、API通信を管理します。
Manages user input, model selection, and API communication.

#### DetectTypeSelector.tsx
5つの検出モード間の切り替えを提供します。
Provides switching between five detection modes.

#### atoms.tsx
Jotaiを使用したグローバル状態管理。
Global state management using Jotai.

---

## 💻 技術スタック / Technology Stack

```mermaid
graph LR
    subgraph "Frontend / フロントエンド"
        React[React 19<br/>UIライブラリ]
        TS[TypeScript<br/>型安全性]
        Vite[Vite<br/>ビルドツール]
        TW[Tailwind CSS<br/>スタイリング]
    end
    
    subgraph "State Management / 状態管理"
        Jotai[Jotai<br/>状態管理]
    end
    
    subgraph "Drawing / 描画"
        PF[Perfect Freehand<br/>手描き線]
        Canvas[HTML5 Canvas<br/>キャンバスAPI]
    end
    
    subgraph "AI / AI"
        Gemini[Google Gemini API<br/>画像認識AI]
    end
    
    subgraph "Utilities / ユーティリティ"
        RRD[React Resize Detector<br/>リサイズ検出]
    end
    
    React --> TS
    React --> Jotai
    React --> PF
    React --> Canvas
    React --> RRD
    Vite --> React
    TW --> React
    React --> Gemini
    
    style React fill:#61DAFB,color:#000
    style TS fill:#3178C6,color:#fff
    style Gemini fill:#4285F4,color:#fff
    style Jotai fill:#000,color:#fff
```

### Dependencies / 依存関係

- **React 19**: Modern UI library
- **TypeScript 5.8**: Type safety and better developer experience
- **Vite 6**: Lightning-fast build tool
- **Jotai 2.10**: Primitive and flexible state management
- **@google/genai 0.7**: Google Gemini AI SDK
- **Tailwind CSS 4**: Utility-first CSS framework
- **Perfect Freehand 1.2**: Draw perfect freehand lines
- **React Resize Detector 12**: Detect element resize

---

## 🚀 セットアップ / Setup

### Prerequisites / 前提条件

- **Node.js** (v18 or higher / v18以上)
- **npm** or **yarn**
- **Google Gemini API Key** / Gemini APIキー

### Installation / インストール

1. **Clone the repository / リポジトリをクローン**
   ```bash
   git clone https://github.com/kenichimiyata/pricecheck.git
   cd pricecheck
   ```

2. **Install dependencies / 依存関係をインストール**
   ```bash
   npm install
   ```

3. **Set up environment variables / 環境変数を設定**
   
   Create a `.env.local` file in the root directory:
   
   `.env.local`ファイルをルートディレクトリに作成:
   ```bash
   GEMINI_API_KEY=your_api_key_here
   ```

4. **Run the development server / 開発サーバーを起動**
   ```bash
   npm run dev
   ```

5. **Open in browser / ブラウザで開く**
   ```
   http://localhost:5173
   ```

### Build for Production / 本番用ビルド

```bash
npm run build
npm run preview
```

---

## 📖 使い方 / Usage

### Basic Workflow / 基本的なワークフロー

```mermaid
flowchart TD
    Start([開始 / Start]) --> Upload[画像をアップロード/選択<br/>Upload/Select Image]
    Upload --> SelectMode[検出モードを選択<br/>Select Detection Mode]
    SelectMode --> Decision{描画が必要？<br/>Need Drawing?}
    
    Decision -->|はい Yes| EnableDraw[描画モードを有効化<br/>Enable Draw Mode]
    Decision -->|いいえ No| EnterPrompt
    
    EnableDraw --> Draw[画像に描画<br/>Draw on Image]
    Draw --> EnterPrompt[プロンプトを入力<br/>Enter Prompt]
    
    EnterPrompt --> ConfigModel[モデル設定を調整<br/>Adjust Model Config]
    ConfigModel --> Send[送信ボタンをクリック<br/>Click Send]
    
    Send --> Processing[AI処理中...<br/>AI Processing...]
    Processing --> Results[結果を表示<br/>Display Results]
    
    Results --> Review{結果を確認<br/>Review Results}
    Review -->|調整 Adjust| ConfigModel
    Review -->|完了 Done| End([終了 / End])
    
    style Start fill:#34A853,color:#fff
    style End fill:#EA4335,color:#fff
    style Processing fill:#FBBC04,color:#000
    style Results fill:#3B68FF,color:#fff
```

### Detection Modes / 検出モード

1. **2D Bounding Boxes**: General object detection
2. **Segmentation Masks**: Detailed object segmentation
3. **Points**: Specific point identification
4. **Price Prediction**: Object detection with price estimation
5. **Text Extraction**: OCR and text recognition

### Tips / ヒント

- **Temperature**: Lower values (0-0.5) for more precise results, higher values (0.6-1.0) for more creative outputs
  
  温度：より正確な結果には低い値（0〜0.5）、よりクリエイティブな出力には高い値（0.6〜1.0）

- **Thinking Mode**: Enable for complex reasoning tasks, disable for faster simple detection
  
  思考モード：複雑な推論タスクには有効化、より高速な単純検出には無効化

- **Drawing**: Use the draw mode to guide the AI by highlighting specific areas
  
  描画：描画モードを使用して、特定の領域を強調表示することでAIを誘導

---

## 📄 License / ライセンス

Copyright 2025 Google LLC

Licensed under the Apache License, Version 2.0

---

## 🤝 Contributing / 貢献

Contributions are welcome! Please feel free to submit a Pull Request.

貢献を歓迎します！お気軽にプルリクエストを提出してください。

---

## 📞 Support / サポート

For issues and questions, please use the GitHub Issues page.

問題や質問については、GitHub Issuesページをご利用ください。
