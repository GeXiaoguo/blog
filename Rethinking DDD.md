# Evolving Perspectives in Domain-Driven Design: From Nouns to Verbs

## Introduction
Domain-Driven Design (DDD) has long been a staple in complex software development, offering a structured approach to dealing with complexities in business requirements. Traditionally, DDD has employed a noun-based approach, focusing on defining aggregates and bounded contexts as key structures. In this noun-based model, the focus is on entities (the 'nouns'), with these entities owning the behaviors (the 'verbs' or methods) that operate within them. However, as our understanding of business use cases evolves, so must our approach to modeling them. Shifting towards a more dynamic, verb-based approach, we start to prioritize business cases themselves. In this verb-based model, business cases are seen as functions, and these functions own the stateful worlds they operate in, rather than being confined by predefined structures. This blog post explores the journey from the traditional noun-based DDD to this more adaptive verb-based approach, highlighting the evolving nature of business cases and the limitations of conventional DDD methods.

## The Evolving Nature of Business Use Cases
Business use cases often start off poorly understood and inevitably evolve over time. This evolution is driven by two factors. 

First, it is inherent in the nature of business itself: as a business develops and adapts to new market trends, competitive pressures, and customer needs, so too do its technical and operational requirements evolve.

Second, these use cases are frequently initiated with limited understanding by both the business and technical teams. This initial gap in understanding means that as the use cases are iterated upon by both teams, more knowledge is acquired, making adjustments and refinements inevitable. 

### Traditional DDD: A Constraint on Evolution
In traditional Domain-Driven Design (DDD), the model is built on the assumption of non-overlapping aggregates and bounded contexts. This design choice, while initially offering clarity, can lead to issues as business requirements evolve. When new requirements necessitate the inclusion of states already captured by other aggregates or bounded contexts, traditional DDD discourages direct state sharing. Instead, it recommends raising domain events to be processed asynchronously by the relevant aggregates/bounded contexts. This approach, while ensuring separation of concerns, can inadvertently create an asynchronous software process to model what is essentially a synchronous business process, overlooking the significant complexity an asynchronous software process adds.

Furthermore, traditional DDD mandates that aggregates own multiple methods (business cases). As these business cases evolve, often independently, the associated stateful worlds they operate in need to adapt as well. This can lead to aggregates growing unwieldy and oversized, or to states being forcibly externalized from the aggregate. The conventional solution of updating these states is, again, with domain events. These events are a byproduct of artificial aggregate boundaries, rather than from domain event storming sessions with the business teams, which is where domain events are meant to be defined.

## Shifting to a Verb-Based Approach
Recognizing these limitations, a shift towards a verb-based approach in DDD is necessary. This approach views each business case as an independent entity that fully owns its operational context. By focusing on behaviors (verbs) rather than entities (nouns), the verb-based model allows for a more organic evolution of business cases, free from the artificial constraints of traditional DDD.

The distinction between these approaches come down to the simple difference below.

Traditional noun-based DDD might look like:
```csharp
class MyAggregateRoot {
    MyStates _myStates;
    void MyBusinessCase1();
    void MyBusinessCase2();
}
```
In this traditional approach:

The `MyAggregateRoot` class encapsulates the state (`MyStates _myStates`) and owns the behaviors `(MyBusinessCase1, MyBusinessCase2)`. Business cases are methods within the aggregate, operating on a shared state. This pattern encourages modeling around entities (nouns) and their lifecycle, with business cases tied to these entities.

In contrast, a verb-based pattern would be represented as:
```csharp
static MyStatefulWorld1 MyBusinessCase1(MyStatefulWorld1 states);
static MyStatefulWorld2 MyBusinessCase2(MyStatefulWorld2 states);
```
In this verb-based approach:

Each business case is represented as a function, emphasizing the action or behavior.
The state (MyStatefulWorld1, MyStatefulWorld2) is passed to the function, and the function operates on this state.

There's a clear separation between the state and the behavior, and each business case is independent, possibly operating on different versions of the state.

## Advantages of the Verb-Based Approach
A verb-based approach in Domain-Driven Design (DDD) presents several compelling advantages, particularly in its flexibility and responsiveness to the evolving nature of the business requirements. 

Firstly, it allows business cases to evolve independently, treating each as a unique function with its own stateful world. This independence means that changes or developments in one area of the business do not necessitate forced adjustments in others, fostering a more agile and adaptive environment.

Moreover, this approach permits the stateful world to evolve more freely. Unlike traditional DDD, where rigid aggregate boundaries can restrict evolution, a verb-based system encourages a more organic growth of the domain model. This growth reflects the actual needs and dynamics of the business, not just predetermined structures.

Verb-based systems still offer robust validation, similar to what aggregates provide. For instance, MyStatefulWorld1 would encompass all necessary validation rules to ensure its integrity, just as an aggregate would. This ensures that while the system is flexible, it does not compromise on the consistency and reliability of the data.

Additionally, verb-based systems allow for the sharing of stateful worlds between functions. If the stateful worlds of two business cases converge, they can be defined as the same type and shared. This sharing is based on practicality and actuality, rather than adhering to a strict design guideline. It's a more pragmatic approach that aligns with how business cases naturally evolve and intersect.

The concept of bounded contexts remains relevant and applicable in a verb-based system. However, these contexts are defined based on actual needs – such as real divisions within business or technical teams – and can evolve as requirements change. This flexibility ensures that bounded contexts are not just theoretical constructs but practical tools that adapt to the shifting realities of the business environment.

## Conclusion
In summary, a verb-based system in DDD brings a dynamic and realistic approach to handling business cases. It combines the advantages of flexibility, independent evolution, and practical bounded contexts, while maintaining the necessary validation and consistency checks, making it a compelling alternative to traditional noun-based DDD in modeling complex and dynamic business scenarios.