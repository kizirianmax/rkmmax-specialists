# 🔗 Guia de Integração - RKMMAX Specialists

Este guia mostra como integrar a biblioteca de especialistas em diferentes projetos.

## 📦 Instalação

### Opção 1: NPM Package (Futuro)

```bash
npm install @rkmmax/specialists
```

### Opção 2: Git Submodule

```bash
cd seu-projeto
git submodule add https://github.com/kizirianmax/rkmmax-specialists.git specialists
```

### Opção 3: Cópia Direta

```bash
# Copiar arquivo para seu projeto
cp specialists/specialists.js seu-projeto/src/config/
```

## 🚀 Uso Básico

### Importar Especialistas

```javascript
import { specialists } from '@rkmmax/specialists';

// ou se copiou direto
import { specialists } from './config/specialists.js';
```

### Listar Todos os Especialistas

```javascript
const allSpecialists = Object.values(specialists);

console.log(`Total: ${allSpecialists.length} especialistas`);

allSpecialists.forEach(specialist => {
  console.log(`${specialist.emoji} ${specialist.name} - ${specialist.description}`);
});
```

### Filtrar por Categoria

```javascript
// Obter todos especialistas de tecnologia
const techSpecialists = Object.values(specialists)
  .filter(s => s.category === 'tech');

// Obter apenas visíveis
const visibleSpecialists = Object.values(specialists)
  .filter(s => s.visible);

// Combinar filtros
const visibleTechSpecialists = Object.values(specialists)
  .filter(s => s.category === 'tech' && s.visible);
```

### Acessar Especialista Específico

```javascript
// Por ID
const code = specialists.code;
console.log(code.systemPrompt);

// Verificar se existe
if (specialists.code) {
  console.log('Especialista Code disponível!');
}
```

## 🎨 Integração com React

### Componente de Card

```jsx
import { specialists } from '@rkmmax/specialists';

function SpecialistCard({ specialistId }) {
  const specialist = specialists[specialistId];
  
  if (!specialist) return null;
  
  return (
    <div className="specialist-card">
      <img src={specialist.avatar} alt={specialist.name} />
      <span className="emoji">{specialist.emoji}</span>
      <h3>{specialist.name}</h3>
      <p>{specialist.description}</p>
      <span className="category">{specialist.category}</span>
    </div>
  );
}

export default SpecialistCard;
```

### Lista de Especialistas

```jsx
import { specialists } from '@rkmmax/specialists';

function SpecialistsList({ category }) {
  const filtered = Object.values(specialists)
    .filter(s => !category || s.category === category)
    .filter(s => s.visible);
  
  return (
    <div className="specialists-grid">
      {filtered.map(specialist => (
        <SpecialistCard 
          key={specialist.id} 
          specialistId={specialist.id} 
        />
      ))}
    </div>
  );
}
```

### Seletor de Categoria

```jsx
import { specialists } from '@rkmmax/specialists';

function CategoryFilter({ onSelect }) {
  // Obter categorias únicas
  const categories = [...new Set(
    Object.values(specialists).map(s => s.category)
  )];
  
  return (
    <select onChange={(e) => onSelect(e.target.value)}>
      <option value="">Todas as categorias</option>
      {categories.map(cat => (
        <option key={cat} value={cat}>
          {cat}
        </option>
      ))}
    </select>
  );
}
```

## 🤖 Integração com APIs de IA

### Gemini API

```javascript
import { specialists } from '@rkmmax/specialists';

async function callGemini(specialistId, userMessage) {
  const specialist = specialists[specialistId];
  
  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-thinking-exp-1219:generateContent?key=${API_KEY}`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        systemInstruction: {
          parts: [{ text: specialist.systemPrompt }]
        },
        contents: [{
          role: 'user',
          parts: [{ text: userMessage }]
        }]
      })
    }
  );
  
  const data = await response.json();
  return data.candidates[0].content.parts[0].text;
}
```

### OpenAI API

```javascript
import { specialists } from '@rkmmax/specialists';

async function callOpenAI(specialistId, userMessage) {
  const specialist = specialists[specialistId];
  
  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${API_KEY}`
    },
    body: JSON.stringify({
      model: 'gpt-4',
      messages: [
        { role: 'system', content: specialist.systemPrompt },
        { role: 'user', content: userMessage }
      ]
    })
  });
  
  const data = await response.json();
  return data.choices[0].message.content;
}
```

### GROQ API

```javascript
import { specialists } from '@rkmmax/specialists';

async function callGroq(specialistId, userMessage) {
  const specialist = specialists[specialistId];
  
  const response = await fetch('https://api.groq.com/openai/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${API_KEY}`
    },
    body: JSON.stringify({
      model: 'llama-3.3-70b-versatile',
      messages: [
        { role: 'system', content: specialist.systemPrompt },
        { role: 'user', content: userMessage }
      ]
    })
  });
  
  const data = await response.json();
  return data.choices[0].message.content;
}
```

## 🎯 Casos de Uso Avançados

### Sistema de Roteamento Inteligente

```javascript
import { specialists } from '@rkmmax/specialists';

function routeToSpecialist(userMessage) {
  // Palavras-chave por categoria
  const keywords = {
    tech: ['código', 'programação', 'bug', 'api', 'deploy'],
    education: ['estudar', 'prova', 'matéria', 'lição'],
    business: ['negócio', 'venda', 'marketing', 'estratégia'],
    // ... mais categorias
  };
  
  // Encontrar categoria mais relevante
  let bestCategory = null;
  let maxMatches = 0;
  
  for (const [category, words] of Object.entries(keywords)) {
    const matches = words.filter(word => 
      userMessage.toLowerCase().includes(word)
    ).length;
    
    if (matches > maxMatches) {
      maxMatches = matches;
      bestCategory = category;
    }
  }
  
  // Retornar primeiro especialista da categoria
  return Object.values(specialists)
    .find(s => s.category === bestCategory && s.visible);
}
```

### Orquestração Multi-Especialista (Estilo Serginho)

```javascript
import { specialists } from '@rkmmax/specialists';

async function orchestrateSpecialists(userTask) {
  // 1. Analisar tarefa e selecionar especialistas relevantes
  const relevantSpecialists = selectRelevantSpecialists(userTask);
  
  // 2. Consultar cada especialista
  const responses = await Promise.all(
    relevantSpecialists.map(specialist =>
      callAI(specialist.id, userTask)
    )
  );
  
  // 3. Sintetizar respostas
  const synthesis = await synthesizeResponses(responses);
  
  return synthesis;
}
```

### Cache de Especialistas

```javascript
import { specialists } from '@rkmmax/specialists';

class SpecialistCache {
  constructor() {
    this.cache = new Map();
  }
  
  get(specialistId) {
    if (!this.cache.has(specialistId)) {
      this.cache.set(specialistId, specialists[specialistId]);
    }
    return this.cache.get(specialistId);
  }
  
  getByCategory(category) {
    return Object.values(specialists)
      .filter(s => s.category === category);
  }
}

const cache = new SpecialistCache();
```

## 📊 Estatísticas e Análises

### Contar Especialistas por Categoria

```javascript
import { specialists } from '@rkmmax/specialists';

function countByCategory() {
  const counts = {};
  
  Object.values(specialists).forEach(specialist => {
    counts[specialist.category] = (counts[specialist.category] || 0) + 1;
  });
  
  return counts;
}

console.log(countByCategory());
// { tech: 7, education: 2, creative: 9, ... }
```

### Gerar Relatório

```javascript
function generateReport() {
  const all = Object.values(specialists);
  const visible = all.filter(s => s.visible);
  const categories = [...new Set(all.map(s => s.category))];
  
  return {
    total: all.length,
    visible: visible.length,
    hidden: all.length - visible.length,
    categories: categories.length,
    byCategory: countByCategory()
  };
}
```

## 🔧 Customização

### Adicionar Metadados Personalizados

```javascript
import { specialists } from '@rkmmax/specialists';

// Estender com dados locais
const extendedSpecialists = Object.fromEntries(
  Object.entries(specialists).map(([id, specialist]) => [
    id,
    {
      ...specialist,
      usage: 0,
      lastUsed: null,
      favorite: false
    }
  ])
);
```

### Tradução

```javascript
const translations = {
  'pt-BR': {
    code: { name: 'Code', description: 'Programação e desenvolvimento' },
    // ...
  },
  'en-US': {
    code: { name: 'Code', description: 'Programming and development' },
    // ...
  }
};

function getLocalizedSpecialist(id, locale = 'pt-BR') {
  const specialist = specialists[id];
  const translation = translations[locale][id];
  
  return {
    ...specialist,
    ...translation
  };
}
```

## 📝 Boas Práticas

1. **Sempre verifique se o especialista existe** antes de usar
2. **Use filtros de visibilidade** para respeitar configurações do usuário
3. **Cache especialistas** quando possível para melhor performance
4. **Valide system prompts** antes de enviar para APIs de IA
5. **Monitore uso** para otimizar custos de API

## 🐛 Troubleshooting

### Especialista não encontrado

```javascript
const specialist = specialists.code;
if (!specialist) {
  console.error('Especialista não encontrado!');
  // Usar especialista padrão ou retornar erro
}
```

### Categoria inválida

```javascript
const validCategories = [
  'education', 'tech', 'creative', 'business', 
  'wellness', 'languages', 'science', 'engineering',
  'lifestyle', 'entertainment'
];

function isValidCategory(category) {
  return validCategories.includes(category);
}
```

## 📞 Suporte

Problemas ou dúvidas? Abra uma issue no [GitHub](https://github.com/kizirianmax/rkmmax-specialists/issues).

---

**Parte do ecossistema RKMMAX** • [Voltar ao README](./README.md)
