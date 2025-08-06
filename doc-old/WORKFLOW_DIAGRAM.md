# 📊 Visual Workflow Diagram

## 🔄 Complete Dataset Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                        AUTO-LABELING TOOL                      │
│                         Main Navigation                        │
│  [Dashboard] [Projects] [Datasets] [Models] [Profile]          │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                         PROJECTS PAGE                          │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   Project   │  │   Project   │  │   Project   │             │
│  │     #1      │  │     #2      │  │     #3      │             │
│  │             │  │             │  │             │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│              [+ Create New Project]                             │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼ (Click Project)
┌─────────────────────────────────────────────────────────────────┐
│                    PROJECT WORKSPACE                           │
│                                                                 │
│ Tabs: [Management] [Dataset] [Versions] [Models] [Visualize]   │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────────┐ │
│ │                    MANAGEMENT TAB                           │ │
│ │                                                             │ │
│ │ ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │ │
│ │ │ UNASSIGNED  │  │ ANNOTATING  │  │   DATASET   │          │ │
│ │ │             │  │             │  │             │          │ │
│ │ │ [Upload]    │  │ Dataset A   │  │ Dataset X   │          │ │
│ │ │ Images      │  │ (3/10 done) │  │ (Complete)  │          │ │
│ │ │             │  │             │  │             │          │ │
│ │ │ Image 1     │  │ Dataset B   │  │ Dataset Y   │          │ │
│ │ │ Image 2     │  │ (0/5 done)  │  │ (Complete)  │          │ │
│ │ │ Image 3     │  │             │  │             │          │ │
│ │ │             │  │             │  │             │          │ │
│ │ └─────────────┘  └─────────────┘  └─────────────┘          │ │
│ │       │                │                │                  │ │
│ │       ▼                ▼                ▼                  │ │
│ │ [Create Dataset]  [Annotate]      [Export/Train]          │ │
│ └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼ (Click Annotate)
┌─────────────────────────────────────────────────────────────────┐
│                    ANNOTATION INTERFACE                        │
│                                                                 │
│ ┌─────────┐ ┌─────────────────────────────┐ ┌─────────────────┐ │
│ │ Image   │ │                             │ │ Tools & Labels  │ │
│ │ List    │ │        Main Canvas          │ │                 │ │
│ │         │ │                             │ │ ┌─────────────┐ │ │
│ │ □ Img1  │ │     [Current Image]         │ │ │ Bounding    │ │ │
│ │ ■ Img2  │ │                             │ │ │ Box         │ │ │
│ │ □ Img3  │ │   ┌─────────────────────┐   │ │ └─────────────┘ │ │
│ │ □ Img4  │ │   │                     │   │ │ ┌─────────────┐ │ │
│ │ □ Img5  │ │   │   Annotation Area   │   │ │ │ Polygon     │ │ │
│ │         │ │   │                     │   │ │ │ Tool        │ │ │
│ │         │ │   └─────────────────────┘   │ │ └─────────────┘ │ │
│ │         │ │                             │ │                 │ │
│ │         │ │   [Zoom] [Pan] [Reset]      │ │ Labels:         │ │
│ │         │ │                             │ │ • Car           │ │
│ │         │ │                             │ │ • Person        │ │
│ │         │ │                             │ │ • Bike          │ │
│ └─────────┘ └─────────────────────────────┘ └─────────────────┘ │
│                                                                 │
│ Progress: [████████░░] 80% Complete                             │
│ [Previous] [Next] [Save] [Complete Dataset]                    │
└─────────────────────────────────────────────────────────────────┘
```

## 🎯 State Transitions

### Image Upload Flow
```
User Action → Upload Images → Unassigned Column
                                    │
                                    ▼
                            [Create Dataset]
                                    │
                                    ▼
                            Named Dataset Created
```

### Dataset Workflow States
```
UNASSIGNED → ANNOTATING → DATASET
    │            │           │
    │            │           ▼
    │            │      [Export/Train]
    │            │
    │            ▼
    │      [Annotation Interface]
    │            │
    │            ▼
    │      Label All Images
    │            │
    │            ▼
    │      Return to Management
    │
    ▼
[Create Dataset] → Move to Annotating
```

## ⚠️ Validation Points

### Critical Checkpoints
```
1. Dataset Creation
   ├─ ✅ Must provide meaningful name
   └─ ❌ No auto-generated names

2. Move to Annotating
   ├─ ✅ Any labeling status allowed
   └─ ✅ Can have 0 labeled images

3. Move to Dataset Column
   ├─ ✅ ALL images must be labeled
   ├─ ❌ Cannot move if any unlabeled
   └─ 🔒 System enforced validation
```

## 📊 Data Flow Architecture

### Frontend → Backend Communication
```
┌─────────────────┐    API Calls    ┌─────────────────┐
│   React UI      │ ──────────────► │   FastAPI       │
│   (Port 12001)  │                 │   (Port 12000)  │
│                 │ ◄────────────── │                 │
│ • Project Mgmt  │    JSON Data    │ • Database      │
│ • File Upload   │                 │ • File Storage  │
│ • Annotation    │                 │ • Path Mgmt     │
└─────────────────┘                 └─────────────────┘
```

### File Storage Structure
```
uploads/
└── projects/
    └── [Project Name]/
        ├── unassigned/
        │   └── [dataset_name]/
        │       ├── image1.jpg
        │       └── image2.jpg
        ├── annotating/
        │   └── [dataset_name]/
        │       ├── image1.jpg
        │       └── image2.jpg
        └── dataset/
            └── [dataset_name]/
                ├── image1.jpg
                └── image2.jpg
```

## 🔄 User Journey Map

### New User Experience
```
1. First Visit
   └─ Dashboard → Overview & Quick Actions

2. Create Project
   └─ Projects Page → Create New → Enter Workspace

3. Upload Data
   └─ Management Tab → Upload Images → Unassigned Column

4. Organize Data
   └─ Select Images → Create Dataset → Name Dataset

5. Start Labeling
   └─ Move to Annotating → Click Annotate → Label Images

6. Complete Dataset
   └─ Finish Labeling → Move to Dataset → Export/Train
```

### Experienced User Flow
```
Dashboard → Recent Project → Upload → Quick Dataset Creation → Annotate → Complete
     │                                                                      │
     └─ Quick Stats & Monitoring                                           │
                                                                           │
Models Page ← Export/Train ← Dataset Tab ← Completed Dataset ←────────────┘
```

## 🎨 UI Component Hierarchy

### Page Structure
```
App.js
├── Navbar
├── Router
│   ├── Dashboard
│   ├── Projects
│   │   └── ProjectWorkspace
│   │       ├── Management Tab
│   │       ├── Dataset Tab
│   │       ├── Versions Tab
│   │       ├── Models Tab
│   │       └── Visualize Tab
│   ├── ManualLabeling
│   │   ├── AnnotationCanvas
│   │   ├── AnnotationToolbox
│   │   ├── LabelSidebar
│   │   └── ImageList
│   ├── Datasets
│   └── Models
└── Footer
```

---

*This diagram shows the complete user workflow and system architecture. For detailed implementation, see UI_DOCUMENTATION.md*