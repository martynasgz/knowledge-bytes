# Contravariance

## Function input contravariance

```ts
type Test<T> = (
  value: any,
  row: T,
  column: Test<T>, // <-- core issue
  apiRef: any
) => any;

const documentImpl: Test<Document> = () => {};
const workspaceImpl: Test<WorkspaceDocument> = documentImpl;
```

A function of the form `(x: X) => void` is assignable to `(y: Y) => void` if `Y` is assignable to `X`, not vice versa.
As an example, `(row: WorkspaceDocument) => void` is assignable to `(row: Document) => void` because `Document` is assignable to `WorkspaceDocument`.  
Whereas if we want to assign a function of form `(column: (row: WorkspaceDocument) => void) => void` to `(column: (row: Document) => void) => void`, `(row: Document) => void` needs to be assignable to `(row: WorkspaceDocument) => void`, but it's not. If we do assign `(row: Document) => void`.  
Example with function implementations:

```ts
type WorkspaceDocument = {workspace_id: string};
type QuoteDocument = {quote_id: string}

type Document = WorkspaceDocument | QuoteDocument;

type WorkspaceFunction = (
  fn: (WorkspaceDocument) => void
) => void;
type GeneralFunction = (
  fn: (Document) => void
) => void;

let workspaceImpl: WorkspaceFunction = (fn: (document: WorkspaceDocument) => void) => {
  // we can call to fn() only with a WorkspaceDocument
};

const generalImpl: GeneralFunction = (fn: (document: Document) => void) => {
  // we can call to fn() with a WorkspaceDocument or QuoteDocument
};

workspaceImpl = generalImpl;
// ^--- TS error! generalImpl expects to be able to call fn() with QuoteDocument too,
// but this violates the WorkspaceFunction type. Any calls to fn() with QuoteDocument in generalImpl
// would result in runtime errors, since we would only pass WorkspaceDocument now.
```

More on this - https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Function_types.
