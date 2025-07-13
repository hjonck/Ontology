
*Hardy Jonck, CEO of Agileworks Group*

---

## **How are we implementing AI in their systems?**

We have had to pivot in how we develop systems completely. For example, during our MoneyWorks accounting/ERP integration, we set out to build an AI-first API and learned some expensive lessons. We have had to pivot to a **"canonical-first"** approach. Instead of building another API that shuttles data around, we've created **44 specialised MCP tools** that understand accounting domain knowledge.

For example, when our AI encounters `AccountType.CURRENT_ASSET`, it doesn't just see a string; it understands this represents liquid resources with specific business implications. We've extracted 30+ years of accounting domain knowledge into **semantic TypeScript definitions** that carry context, not just data.

The result? Our AI can have conversations like *"Show me accounts that affect cash flow"* rather than requiring technical queries like *"SELECT \* WHERE account\_type \= 'CA'"*.

---

## **What is the biggest challenge you have had to overcome?**

**Our biggest challenge was not realising earlier that we had to reconsider our approach to building software for an AI-first architecture completely.** We initially tried the traditional route—building APIs and then writing elaborate context-specific prompts to help AI understand our data. We quickly discovered we were burning tokens up the wrong tree, spending enormous effort on prompt engineering to compensate for semantically poor data structures.

But the deeper challenge was **ontological mapping**. MoneyWorks has fields like `Type` that could mean account type, transaction type, or contact type, depending on context. We had to systematically extract and codify three decades of accounting domain knowledge into machine-readable semantic definitions, originally scattered in a human-friendly, glorious 800-page MoneyWorks manual.

It forced us to ask: *Does this field exist because of accounting principles, business logic, or just historical accident?* That questioning revealed where the real intelligence lived—and it wasn't in our databases.

---

## **What benefit do you hope to reap from this integration?**

**Democratisation of complex accounting knowledge.** Today, you need years of MoneyWorks training to navigate its 1000+ fields and 30+ tables. Our AI can bridge that gap.

Imagine asking: *"Why is my gross margin declining?"* and getting contextual analysis that connects product costs, account classifications, and transaction patterns—not just numbers, but the story they tell.

We're moving from **"Show me the data"** to **"Help me understand the business."** That's the difference between information and intelligence.

---

## **What does it look like when a system understands why a piece of data exists, not just what it is?**

Take our `PaymentMethod` enum. Traditional systems store `"CC"` as a string. Our canonical ontology knows:

- `PaymentMethod.CREDIT_CARD` has merchant fees  
- It affects cash flow timing differently than `PaymentMethod.CASH`  
- It requires different reconciliation processes  
- It has different fraud risk profiles

When AI encounters this data, it doesn't just see payment type—it understands the **business implications, relationships, and constraints** that the payment method carries. The data becomes **self-describing and context-aware**.

---

## **How can a company tell if its current stack is built on the "half-truth" of data being just data?**

**Red flags we discovered:**

- **Magic numbers everywhere**: Status codes like `1`, `2`, `3` with no semantic meaning  
- **Tribal knowledge dependency**: Only Janet knows why certain fields matter  
- **Documentation debt**: APIs that describe *what* but never *why*  
- **AI confusion**: Your models can't explain their reasoning because the data can't explain itself

**The litmus test**: Can a new team member understand your business logic just by reading your data structures? If not, your AI certainly can't.

---

## **How should teams shift their thinking when approaching AI-first architecture?**

**Start with domain modelling, not data modelling.** We began by asking: *"How does an accountant think about this information?"* not *"How do we store this data?"*

**Traditional flow**: Database → API → Interface → AI (bolted on)  
**Our approach**: Domain Understanding → Semantic Models → Context-Aware APIs → AI-Native Tools

**Practical shifts:**

- Replace primitive types with semantic enums  
- Build canonical ontologies before schemas  
- Design for AI comprehension, not just human consumption  
- Think relationships and meaning first, storage optimisation second

---

## **Do you think most companies will ever be ready for AI that's truly intelligent, or will we always be retrofitting meaning onto legacy systems?**

**The honest answer: Both paths will coexist, but the winners will be those who rebuild.**

Retrofitting can work for basic automation—chatbots reading FAQs, simple data queries. But **transformative AI** requires semantic foundations. You can't bolt meaning onto systems designed around meaningless identifiers.

**The competitive advantage** belongs to organisations willing to rebuild their information architecture around meaning. Those who do will have AI that truly understands their business. Those who don't will have very expensive data retrievers.

**Our MoneyWorks project proves it's possible**—but it requires treating semantic design as seriously as technical architecture. The companies that get this right won't just have better AI; they'll have **AI-native competitive advantages** that are impossible to replicate with legacy retrofits.

---

*The future belongs to organisations where every piece of data carries its own intelligence. We're not just building better APIs—we're building systems that can think.*