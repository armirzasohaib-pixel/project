# MODULAR CUTLIST GENERATOR

_Modular Furniture Automation Platform (Kitchen + All Furniture Types)_

## 1) Goal
Build a software platform where a designer/factory uploads:
- Plan
- Elevation
- Section
- Internal elevations
- Material selections
- 3D model

…and the system automatically generates production-ready outputs for **any modular furniture type and layout**, including:
- Kitchens (L, U, Parallel, Straight, Island, G-shape, open kitchens)
- Wardrobes (straight, L-corner, walk-in)
- TV units
- Crockery/bar units
- Study/workstation units
- Vanity and bathroom storage
- Utility/laundry units
- Office modular furniture

Required outputs:
- Cutlist (Excel)
- Nesting cut plan (PDF)
- Panel drawings (PDF)
- Hardware BOQ (India-market SKUs)
- CNC-ready files (optional phase)

---

## 2) Core Product Vision
A cloud + factory workflow product with these layers:
1. **Design Intake** (uploads + metadata capture)
2. **Space & Object Interpretation Engine** (room + furniture decomposition)
3. **Rule & Standards Engine** (furniture-type and layout rules)
4. **Hardware Recommendation Engine** (India SKU catalog)
5. **Manufacturing Engine** (panelization, cutlist, nesting)
6. **Output Generator** (PDF/Excel/3D review)
7. **Job Tracking, Revision, and Approval Control**

---

## 3) High-Level Architecture

```text
[Web App / API]
   |
   +-- Project Service
   +-- File Ingestion Service (CAD/PDF/IFC/OBJ/GLTF)
   +-- Design Validation Service
   +-- Parametric Furniture Service
   +-- Rule Engine Service
   +-- Hardware Catalog Service
   +-- BOM/Cutlist Service
   +-- Nesting Service
   +-- Drawing Generation Service
   +-- Export Service (PDF/XLSX/DXF/CSV)
   +-- Notification Service
   |
[PostgreSQL + Object Storage + Queue + Cache]
   |
[CNC / ERP / Factory Integration Layer]
```

### Recommended stack
- **Frontend**: React + TypeScript + Three.js viewer
- **Backend**: Python (FastAPI) or Node.js (NestJS)
- **Geometry/Nesting**: Python + computational geometry libs (or C++ module)
- **DB**: PostgreSQL
- **File storage**: S3-compatible object storage
- **Queue**: RabbitMQ / Redis streams
- **Document generation**: ReportLab / wkhtmltopdf / LibreOffice headless

---

## 4) Functional Modules

### A. Design Intake Module
- Upload files: DWG/DXF, PDF, IFC, SKP/OBJ/GLTF (configurable)
- Capture project parameters:
  - Furniture category (kitchen/wardrobe/TV/study/etc.)
  - Layout style and room constraints
  - Wall lengths/heights, ceiling, offsets, beams
  - Material board sizes and thicknesses
  - Grain direction preferences
  - Edge-banding rules

### B. Parametric Furniture Decomposition Module
- Identify walls/corners/openings/services
- Split design into modules:
  - Carcass units
  - Shutters/fronts
  - Drawers
  - Shelves
  - Loft/top units
  - Special accessories (pull-out pantry, trouser pull-out, shoe rack, etc.)
- Support parametric families for **all modular furniture categories**

### C. Rule Engine
- Apply dimension constraints and design standards by furniture type:
  - Kitchens: hob/sink/corner rules
  - Wardrobes: hanging depth, shutter splits, loft clearances
  - TV/Display units: cable/service clearances
  - Workstations: ergonomic desk heights/depths
- Versioned rule packs (factory-specific)

### D. Hardware Engine (India)
- Maintain vendor catalogs:
  - Hettich, Hafele, Blum, Ebco, Grass, Godrej, Dorset, etc.
- Auto-map hardware by furniture/unit type:
  - Hinges, channels, tandem drawers, lift-ups, pantry pull-outs, profile handles, locks
- Output SKU-wise BOM with brand alternatives and substitution rules

### E. Manufacturing Engine
1. Convert modular units to panels/components
2. Apply material + edge rules
3. Generate cutlist (dimensions, qty, edgeband, grain, material code)
4. Run nesting optimization by sheet size/material
5. Produce panel IDs + labels/barcodes + machining metadata

### F. Output Module
- **Cutlist Excel (.xlsx)**
- **Nesting Plan PDF**
- **Panel Drawings PDF** (shop drawings with dimensions)
- **Hardware BOQ PDF/Excel**
- **3D approval view** for client and production
- Optional DXF/CNC export pack

### G. Revision & Approval Workflow
- Version each upload and output set
- “Design approved for production” gate
- Delta output (what changed from previous revision)
- Approval roles: designer → reviewer → factory planner

---

## 5) Suggested Data Model (Core Entities)
- `Project`
- `ProjectRevision`
- `UploadedFile`
- `Space`
- `Layout`
- `FurnitureUnit`
- `ComponentPanel`
- `Material`
- `HardwareItem`
- `BOMLine`
- `CutlistLine`
- `NestSheet`
- `DrawingPack`
- `ExportArtifact`

---

## 6) Processing Pipeline
1. User creates project and uploads design inputs
2. Parser normalizes geometry across file formats
3. Rules engine validates and flags issues
4. Parametric generator builds modular furniture units
5. Hardware engine maps fittings + accessories
6. Panelizer creates all board parts
7. Nesting optimizer generates sheet-wise cuts
8. Export engine produces PDF/XLSX/DXF deliverables
9. User reviews, revises, and approves for production

---

## 7) API Structure (Example)
- `POST /projects`
- `POST /projects/{id}/uploads`
- `POST /projects/{id}/revisions/{revId}/process`
- `GET /projects/{id}/outputs`
- `GET /projects/{id}/outputs/cutlist.xlsx`
- `GET /projects/{id}/outputs/nesting.pdf`
- `GET /projects/{id}/outputs/panel-drawings.pdf`
- `GET /projects/{id}/outputs/hardware-boq.xlsx`
- `GET /catalog/hardware?vendor=hettich`
- `GET /catalog/furniture-families?type=wardrobe`

---

## 8) UI Screens
1. Dashboard (projects + status)
2. New Project Wizard
3. Upload & Drawing/Model Mapping Screen
4. Validation Issues Screen
5. 3D Review + Material/Hardware selector
6. Outputs Download Center
7. Admin Catalog Manager (materials/hardware/rules/families)

---

## 9) Implementation Phases

### Phase 1 (MVP)
- Upload + manual parameter capture
- Kitchen + wardrobe + TV unit support
- Cutlist XLSX + simple nesting PDF
- Hardware BOQ from preset templates

### Phase 2
- Automated extraction from drawings/3D
- Advanced nesting optimization
- Panel drawings with annotations
- Revision compare and approval workflow

### Phase 3
- CNC machine integration (DXF/G-code connectors)
- Multi-factory setup
- Costing, lead time, and procurement integration
- ERP and inventory sync

---

## 10) Non-Functional Requirements
- Multi-tenant security
- Audit logs for revisions
- Asynchronous processing for heavy jobs
- Robust error handling for malformed CAD files
- Localized units and standards for Indian market
- Library versioning for hardware/material catalogs

---

## 11) Team Structure Needed
- Product manager (manufacturing domain)
- CAD/geometry engineer
- Backend engineer(s)
- Frontend engineer
- QA + factory validation specialist
- Catalog/data operations (materials + hardware SKUs)

---

## 12) Final Deliverables Coverage
This architecture is designed to generate outputs for:
- **Any modular furniture layout/type** (not only kitchens)
- India-market modern hardware inclusion
- Nesting cut plan PDF
- Panel drawing PDF
- Cutlist Excel
- End-to-end upload-to-production workflow
