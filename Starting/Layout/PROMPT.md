# Resonant.ai - Layout Documentation

## Mission

Créer une documentation complète et visuelle de tous les layouts de l'application Resonant.ai, une plateforme de création de workflows AI visuels inspirée de Relevance AI et Poppy AI.

## Contexte

Resonant.ai est une plateforme SaaS qui permet de créer visuellement des workflows d'IA en connectant des blocs (comme un DAG dans React Flow). L'application comprend:

- **Dashboard**: Vue d'ensemble avec stats, recent workflows, activity
- **Workflow Editor**: Éditeur ReactFlow avec canvas, palette de blocs, panels
- **Templates/Marketplace**: Galerie de templates et marketplace communautaire
- **Settings**: Configuration workspace, billing, AI providers
- **Collaboration**: Temps réel avec Yjs, commentaires, partage

## Stack Technique

- Next.js 16 + React 19
- shadcn/ui + Tailwind CSS 4
- React Flow pour l'éditeur
- Lucide Icons uniquement
- Inter (texte) + Space Mono (code)

## Style de Documentation

Chaque fichier doit contenir:

1. **Header**: Titre, description, breadcrumb
2. **Wireframe ASCII**: Représentation visuelle en ASCII art
3. **Zones identifiées**: Liste des zones avec dimensions/positions
4. **Components utilisés**: shadcn/ui components nécessaires
5. **États**: Normal, loading, empty, error
6. **Responsive**: Adaptations mobile/tablet
7. **Interactions**: Hover, click, drag, keyboard
8. **Liens**: Vers les pages/composants liés

## Format ASCII Art

Utiliser ce style pour les wireframes:

```
┌─────────────────────────────────────────────────────────────────┐
│ HEADER                                                          │
├──────────┬──────────────────────────────────────────────────────┤
│          │                                                       │
│ SIDEBAR  │              MAIN CONTENT                            │
│          │                                                       │
│          │  ┌─────────┐  ┌─────────┐  ┌─────────┐              │
│          │  │  CARD   │  │  CARD   │  │  CARD   │              │
│          │  └─────────┘  └─────────┘  └─────────┘              │
│          │                                                       │
└──────────┴──────────────────────────────────────────────────────┘
```

## Références

- Relevance AI: https://relevanceai.com
- Poppy AI: https://poppy.ai (workflow builder)
- shadcn/ui: https://ui.shadcn.com
- React Flow: https://reactflow.dev

## Contraintes

- Documentation uniquement (pas de code)
- ASCII art obligatoire pour chaque layout
- Ultra-complet: toutes les features des 400 steps
- Fichiers en Markdown
- Liens croisés entre les documents
