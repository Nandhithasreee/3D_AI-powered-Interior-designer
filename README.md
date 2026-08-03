# InDwell — Reside Inside Your Vision

> **An AI-powered interior design platform that transforms natural-language ideas into structured room designs and immersive 3D experiences.**

InDwell is a full-stack **Generative AI interior design platform** designed to let users describe a room in natural language and turn that description into an intelligent, visually rich interior concept.

The core AI workflow uses a **Large Language Model (LLM)** to understand and enhance the user's description, optimize the prompt for image generation, and produce structured room data. The resulting scene data can be consumed by **Three.js and A-Frame** to create an interactive 3D room experience.

The frontend is built with **React, Tailwind CSS, Framer Motion, GSAP, Anime.js, Three.js/A-Frame-compatible scene rendering, and React Router**. The backend architecture is designed around **Django and Django REST Framework**, with provider-independent LLM and image-generation services.

---

## Features

### AI-Powered Prompt Enhancement

Users can enter a simple description such as:

```text
Modern bedroom
```

The LLM enhances it into a more detailed interior-design prompt containing relevant design information such as:

* Room style
* Furniture
* Colors
* Materials
* Lighting
* Atmosphere
* Layout
* Decorative elements

Example:

```text
Design a modern Scandinavian bedroom with warm oak flooring,
beige walls, a king-size upholstered bed, soft ambient lighting,
large windows, indoor plants, minimalist furniture and natural daylight.
```

---

### Automatic Prompt Optimization

The system is designed to improve incomplete or very short prompts while preserving the user's original intent.

```text
User Input
    ↓
LLM
    ↓
Enhanced / Optimized Prompt
    ↓
Image Generation Model
```

This allows users to describe a room naturally without needing advanced prompt-engineering knowledge.

---

### Structured Room Planning

In addition to the optimized prompt, the LLM generates structured room information that can be consumed by the frontend's 3D scene system.

Example:

```json
{
  "room_type": "Bedroom",
  "style": "Scandinavian",
  "dimensions": {
    "length": 5,
    "width": 4
  },
  "wall_color": "Warm White",
  "floor_material": "Oak Wood",
  "lighting": [
    "Ceiling Lights",
    "Bedside Lamps"
  ],
  "furniture": [
    {
      "name": "King Bed",
      "position": "Center"
    },
    {
      "name": "Wardrobe",
      "position": "Left Wall"
    }
  ]
}
```

The structured scene is intended to provide the information required for interactive room visualization.

---

### Interactive 3D Room Visualization

InDwell includes a live room visualization system that interprets structured scene data and creates a navigable 3D environment.

The viewer supports concepts such as:

* Camera movement
* Rotation
* Zoom
* Room exploration
* Furniture placement
* Room dimensions
* Material and color representation
* Immersive scene presentation

The frontend uses **A-Frame-based 3D rendering**, with scene data structured so that Three.js can also be used for advanced rendering and controls.

> **Note:** A standard AI-generated 2D image is not automatically a true 3D model. InDwell therefore separates the photorealistic image-generation layer from the structured scene-rendering layer.

---

### Multi-Provider AI Architecture

InDwell is intentionally designed **not to depend on a single LLM or AI provider**.

The backend uses provider abstractions so that different services can be configured without changing the application's core AI logic.

#### LLM providers

The architecture can support providers such as:

* Google Gemini
* OpenAI
* Anthropic Claude
* Local models through Ollama
* Other compatible LLM APIs

#### Image-generation providers

The architecture can support:

* OpenAI image generation
* Fal.ai
* Stability AI
* Replicate
* Other compatible image-generation services

The provider layer allows the application to switch providers or implement fallback logic without rewriting the rest of the application.

---

# System Architecture

```text
                         InDwell
                            │
                            ▼
                    React Frontend
                            │
                            │ REST API
                            ▼
                Django + Django REST Framework
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
          LLM Provider            Image Provider
                │                       │
                ▼                       ▼
       Prompt Enhancement       Photorealistic Image
       Prompt Optimization
       Structured Room JSON
                │                       │
                └───────────┬───────────┘
                            ▼
                   Structured Design
                            │
                            ▼
               Three.js / A-Frame Viewer
                            │
                            ▼
                  Interactive Room Scene
```
---

# 3D Scene Rendering

The frontend contains `RoomViewer3D.jsx`, which interprets structured room data and renders the scene.

The rendering layer is responsible for:

* Room geometry
* Furniture geometry
* Scene colors
* Dimensions
* Camera configuration
* Lighting
* User interaction

The backend should **provide scene data, not render the scene itself**.

---


### Anime.js

Used for:

* SVG/vector motion
* Micro-interactions
* Icon and decorative animations

Animations should remain smooth and performance-conscious.

---

# Running the Frontend

## Prerequisites

Install:

* Node.js
* npm

Then:

```bash
cd frontend
npm install
```

Start the development server:

```bash
npm run dev
```

Open:

```text
http://localhost:5173
```

---

# Frontend Environment

Create a `.env` file based on `.env.example`.

Example:

```env
VITE_API_BASE_URL=http://127.0.0.1:8000/api
```

The exact value should match the Django backend URL.

> In the current standalone frontend, the mock service layer does not require a live backend.

---

# Running the Django Backend

Create and activate a virtual environment:

Install dependencies:

```bash
pip install -r requirements.txt
```

Create the environment file.

Configure the required environment variables.

Run migrations:

```bash
python manage.py migrate
```

Start Django:

```bash
python manage.py runserver
```

The backend will normally be available at:

```text
http://127.0.0.1:8000/
```

---

# AI Provider Configuration

The backend should use environment variables rather than hard-coded API keys.

Example:

```env
DJANGO_SECRET_KEY=your-secret-key

GEMINI_API_KEY=
OPENAI_API_KEY=
ANTHROPIC_API_KEY=

FAL_API_KEY=
REPLICATE_API_KEY=

REDIS_URL=redis://127.0.0.1:6379/0

DATABASE_URL=
```

Only configure the providers you intend to use.

The provider architecture allows the application to remain independent of a single AI vendor.

---