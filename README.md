# 🤖 RKMMAX Specialists

Biblioteca modular com **54 especialistas de IA** para o ecossistema RKMMAX.

## 📋 Sobre

Este repositório contém a configuração de todos os 54 agentes especializados usados na plataforma RKMMAX. Cada especialista tem:

- **ID único**: Identificador do agente
- **Nome e emoji**: Identidade visual
- **Categoria**: Organização por área
- **Descrição**: Resumo das capacidades
- **System Prompt**: Instruções específicas para o modelo de IA
- **Visibilidade**: Controle de exibição na interface

## 🎯 Especialistas por Categoria

### 📚 Educação (2)
- **Didak**: Especialista em didática e métodos de ensino
- **Edu**: Tutor acadêmico para todas as matérias

### 💻 Tecnologia (7)
- **Code**: Programação e desenvolvimento
- **Nexus**: Redes e infraestrutura
- **Synth**: Inteligência Artificial e Machine Learning
- **Shield**: Segurança cibernética
- **Data**: Ciência de dados e análise
- **Cloud**: Cloud computing e DevOps
- **UX**: Design de experiência do usuário

### 🎨 Criatividade (9)
- **Arte**: Arte visual e design gráfico
- **Muse**: Escrita criativa e storytelling
- **Melody**: Música e composição
- **Lens**: Fotografia e edição de imagens
- **Frame**: Cinema e produção audiovisual
- **Stage**: Teatro e performance
- **Pixel**: Game design e desenvolvimento de jogos
- **Brand**: Branding e identidade visual
- **Motion**: Motion design e animação

### 💼 Negócios (7)
- **Strat**: Estratégia empresarial
- **Market**: Marketing digital
- **Sales**: Vendas e negociação
- **Finance**: Finanças e investimentos
- **HR**: Recursos humanos
- **Legal**: Consultoria jurídica
- **Ops**: Operações e logística

### 💙 Bem-estar (5)
- **Fit**: Fitness e exercícios
- **Nutri**: Nutrição e alimentação
- **Mind**: Saúde mental e mindfulness
- **Coach**: Coaching e desenvolvimento pessoal
- **Zen**: Produtividade e organização

### 🌍 Idiomas (3)
- **Poly**: Aprendizado de idiomas
- **Translate**: Tradução profissional
- **English**: Professor de inglês

### 🔬 Ciências (5)
- **Bio**: Biologia e ciências da vida
- **Chem**: Química
- **Physics**: Física
- **Math**: Matemática
- **Geo**: Geografia e geociências

### 🔧 Engenharia (3)
- **Mech**: Engenharia mecânica
- **Electro**: Engenharia elétrica
- **Civil**: Engenharia civil

### ✨ Lifestyle (5)
- **Travel**: Viagens e turismo
- **Fashion**: Moda e estilo
- **Home**: Decoração e design de interiores
- **Green**: Sustentabilidade e meio ambiente
- **Pet**: Cuidados com animais de estimação

### 🎮 Extras (8)
- **Game**: Estratégias e dicas de jogos
- **Anime**: Cultura anime e mangá
- **Book**: Recomendações literárias
- **Food**: Culinária e receitas
- **Wine**: Enologia e harmonização
- **Tech News**: Notícias de tecnologia
- **Crypto**: Criptomoedas e blockchain
- **Space**: Astronomia e exploração espacial

## 📦 Instalação

### Como dependência npm (futuro)

```bash
npm install @rkmmax/specialists
```

### Uso direto

```javascript
import { specialists } from './specialists.js';

// Acessar todos os especialistas
console.log(Object.keys(specialists).length); // 54

// Acessar especialista específico
const code = specialists.code;
console.log(code.name); // "Code"
console.log(code.description); // "Programação e desenvolvimento"

// Filtrar por categoria
const techSpecialists = Object.values(specialists)
  .filter(s => s.category === 'tech');
console.log(techSpecialists.length); // 7

// Obter apenas especialistas visíveis
const visibleSpecialists = Object.values(specialists)
  .filter(s => s.visible);
```

## 🔧 Estrutura de um Especialista

```javascript
{
  id: 'code',                    // Identificador único
  name: 'Code',                  // Nome do especialista
  emoji: '💻',                   // Emoji representativo
  avatar: '/avatars/code.png',   // Caminho do avatar
  category: 'tech',              // Categoria
  description: 'Programação e desenvolvimento',  // Descrição curta
  systemPrompt: 'Você é Code...', // Instruções para a IA
  visible: true                  // Visibilidade na interface
}
```

## 🎯 Categorias Disponíveis

- `education` - Educação
- `tech` - Tecnologia
- `creative` - Criatividade
- `business` - Negócios
- `wellness` - Bem-estar
- `languages` - Idiomas
- `science` - Ciências
- `engineering` - Engenharia
- `lifestyle` - Estilo de vida
- `entertainment` - Entretenimento

## 🚀 Integração com RKMMAX

Este repositório é usado pelo [RKMMAX Híbrido](https://github.com/kizirianmax/rkmmax-hibrido) como fonte de especialistas.

### No app principal:

```javascript
import { specialists } from '@rkmmax/specialists';

// Usar em componentes React
function SpecialistCard({ specialistId }) {
  const specialist = specialists[specialistId];
  
  return (
    <div>
      <span>{specialist.emoji}</span>
      <h3>{specialist.name}</h3>
      <p>{specialist.description}</p>
    </div>
  );
}
```

## 📝 Adicionando Novos Especialistas

1. Edite `specialists.js`
2. Adicione novo objeto com a estrutura padrão
3. Escolha categoria apropriada
4. Crie system prompt específico
5. Defina `visible: true` quando pronto

```javascript
newSpecialist: {
  id: 'newSpecialist',
  name: 'Nome do Especialista',
  emoji: '🎯',
  avatar: '/avatars/newSpecialist.png',
  category: 'categoria',
  description: 'Descrição curta',
  systemPrompt: 'Você é [Nome], especialista em [área]...',
  visible: true,
}
```

## 🔄 Versionamento

Seguimos [Semantic Versioning](https://semver.org/):

- **MAJOR**: Mudanças incompatíveis na estrutura
- **MINOR**: Novos especialistas ou funcionalidades
- **PATCH**: Correções e melhorias em especialistas existentes

## 📊 Estatísticas

- **Total de especialistas**: 54
- **Categorias**: 10
- **Idiomas suportados**: Português (primário), Inglês, Espanhol
- **Modelos de IA compatíveis**: Gemini 2.0, GPT-4, Claude, Llama

## 🤝 Contribuindo

Contribuições são bem-vindas! Para adicionar ou melhorar especialistas:

1. Fork este repositório
2. Crie uma branch (`git checkout -b feature/novo-especialista`)
3. Faça suas mudanças
4. Commit (`git commit -m 'feat: adiciona especialista X'`)
5. Push (`git push origin feature/novo-especialista`)
6. Abra um Pull Request

## 📄 Licença

Proprietary - © 2025 RKMMAX. Todos os direitos reservados.

## 📞 Contato

**Autor**: Roberto Kiziriam Max  
**Email**: roberto@kizirianmax.site  
**Website**: https://kizirianmax.site

---

<div align="center">

**Parte do ecossistema RKMMAX**

[RKMMAX Híbrido](https://github.com/kizirianmax/rkmmax-hibrido) • [Website](https://kizirianmax.site)

</div>
