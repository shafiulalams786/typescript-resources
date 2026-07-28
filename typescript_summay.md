# TypeScript শেখা — বাংলা টিউটোরিয়াল (এখন পর্যন্ত)

এই ফাইলে এখন পর্যন্ত আলোচনা করা সবকিছু একসাথে রাখা হলো — বেসিক থেকে শুরু করে Generics-এর গভীর ও এক্সটেন্ডেড প্রজেক্ট উদাহরণ পর্যন্ত।

## সূচিপত্র

**পার্ট ১: মূল টিউটোরিয়াল**
- ধাপ ১-১৪: বেসিক টাইপ থেকে Next.js/Vue ইন্টিগ্রেশন পর্যন্ত

**পার্ট ২: Generics — বিস্তারিত আলোচনা**
- Generics কেন দরকার, constraints, keyof, ৩টি প্রজেক্ট উদাহরণ (API Client, Repository, Form Validator)

**পার্ট ৩: Generics প্রজেক্ট — Extended ভার্সন**
- Retry/Cache/Auth সহ API Client, Filter/Sort/Pagination সহ Repository, Async validation সহ Form Validator

---

# পার্ট ১: মূল টিউটোরিয়াল

শূন্য থেকে শুরু করে অ্যাডভান্সড লেভেল পর্যন্ত, কোড উদাহরণসহ।

---

## ধাপ ১: TypeScript কী এবং কেন দরকার?

TypeScript হলো JavaScript-এর উপর তৈরি একটি "সুপারসেট" ভাষা। এতে JavaScript-এর সব ফিচার আছে, সাথে যোগ হয়েছে **Static Typing** (টাইপ চেকিং)।

**সমস্যা (শুধু JavaScript দিয়ে):**
```javascript
function add(a, b) {
  return a + b;
}

add(5, "10"); // ভুল হলেও কোনো error দেখাবে না, রেজাল্ট হবে "510"
```

**সমাধান (TypeScript দিয়ে):**
```typescript
function add(a: number, b: number): number {
  return a + b;
}

add(5, "10"); // ❌ Compile-time এ error: Argument of type 'string' is not assignable
```

TypeScript কোড রান করার আগেই ভুল ধরিয়ে দেয়, যা বড় প্রজেক্টে (যেমন তোমার Laravel/Next.js/Vue প্রজেক্ট) খুবই গুরুত্বপূর্ণ।

---

## ধাপ ২: ইনস্টলেশন ও সেটআপ

```bash
# Node.js প্রজেক্টে TypeScript ইনস্টল করা
npm init -y
npm install typescript --save-dev

# tsconfig.json তৈরি করা
npx tsc --init
```

`tsconfig.json` এর কিছু গুরুত্বপূর্ণ অপশন:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "esModuleInterop": true
  }
}
```

**Compile ও রান করা:**
```bash
npx tsc          # সব .ts ফাইল compile করে .js বানাবে
node dist/index.js
```

দ্রুত টেস্ট করার জন্য `ts-node` ব্যবহার করতে পারো:
```bash
npm install -g ts-node
ts-node src/index.ts
```

---

## ধাপ ৩: Basic Types

```typescript
// প্রাইমিটিভ টাইপ
let age: number = 25;
let name: string = "Rahim";
let isActive: boolean = true;

// Array
let scores: number[] = [90, 85, 70];
let names: Array<string> = ["Karim", "Salma"];

// Tuple — ফিক্সড লেংথ ও টাইপের array
let person: [string, number] = ["Rahim", 25];

// Any — যেকোনো টাইপ (এড়িয়ে চলা ভালো)
let anything: any = "যা খুশি তাই";

// Unknown — any এর চেয়ে নিরাপদ
let value: unknown = 10;

// Void — যে function কিছু রিটার্ন করে না
function log(message: string): void {
  console.log(message);
}

// Null ও Undefined
let empty: null = null;
let notDefined: undefined = undefined;
```

---

## ধাপ ৪: Interface ও Type

দুইটাই অবজেক্টের "আকৃতি" (shape) নির্ধারণ করতে ব্যবহার হয়।

```typescript
// Interface
interface User {
  id: number;
  name: string;
  email: string;
  isAdmin?: boolean; // ? মানে অপশনাল প্রপার্টি
}

const user: User = {
  id: 1,
  name: "Rahim",
  email: "rahim@example.com"
};

// Type Alias
type Product = {
  id: number;
  title: string;
  price: number;
};

const product: Product = {
  id: 101,
  title: "Laptop",
  price: 50000
};
```

**Interface vs Type — পার্থক্য:**
- Interface পরে `extends` করে বাড়ানো যায়, একই নামে দুইবার declare করলে merge হয়ে যায়।
- Type দিয়ে union, intersection করা যায় (interface দিয়ে যায় না)।

```typescript
// Interface extends
interface Admin extends User {
  permissions: string[];
}

// Type union
type Status = "active" | "inactive" | "banned";
```

---

## ধাপ ৫: Function-এ টাইপিং

```typescript
// প্যারামিটার ও রিটার্ন টাইপ
function multiply(a: number, b: number): number {
  return a * b;
}

// Optional প্যারামিটার
function greet(name: string, title?: string): string {
  return title ? `${title} ${name}` : name;
}

// Default প্যারামিটার
function createUser(name: string, role: string = "user") {
  return { name, role };
}

// Rest প্যারামিটার
function sum(...numbers: number[]): number {
  return numbers.reduce((acc, n) => acc + n, 0);
}

// Arrow function
const divide = (a: number, b: number): number => a / b;

// Function type হিসেবে ভেরিয়েবল
let calculate: (a: number, b: number) => number;
calculate = add;
```

---

## ধাপ ৬: Union, Intersection ও Literal Types

```typescript
// Union — একাধিক টাইপের যেকোনো একটি
function printId(id: number | string) {
  console.log(`ID: ${id}`);
}

// Literal type — নির্দিষ্ট কিছু ভ্যালু ছাড়া কিছু নেওয়া যাবে না
type Direction = "up" | "down" | "left" | "right";

function move(dir: Direction) {
  console.log(`Moving ${dir}`);
}

move("up");    // ✅ ঠিক আছে
// move("north"); // ❌ Error

// Intersection — একাধিক টাইপ একসাথে মিশিয়ে
type Person = { name: string };
type Employee = { salary: number };

type StaffMember = Person & Employee;

const staff: StaffMember = { name: "Karim", salary: 30000 };
```

---

## ধাপ ৭: Class ও OOP

```typescript
class Animal {
  // প্রপার্টি
  protected name: string;
  private age: number;
  readonly species: string;

  constructor(name: string, age: number, species: string) {
    this.name = name;
    this.age = age;
    this.species = species;
  }

  makeSound(): void {
    console.log(`${this.name} একটি শব্দ করছে`);
  }
}

// Inheritance
class Dog extends Animal {
  constructor(name: string, age: number) {
    super(name, age, "Dog");
  }

  makeSound(): void {
    console.log(`${this.name} ঘেউ ঘেউ করছে`);
  }
}

const dog = new Dog("Tommy", 3);
dog.makeSound();
```

**Access Modifiers:**
- `public` — সবজায়গা থেকে অ্যাক্সেস করা যায় (ডিফল্ট)
- `private` — শুধু ঐ class-এর ভেতরে
- `protected` — ঐ class ও child class-এ

```typescript
// Shorthand constructor (খুব common pattern)
class Product {
  constructor(
    public id: number,
    public title: string,
    private price: number
  ) {}

  getPrice(): number {
    return this.price;
  }
}
```

**Interface দিয়ে Class implement করা:**
```typescript
interface Shape {
  area(): number;
}

class Circle implements Shape {
  constructor(private radius: number) {}

  area(): number {
    return Math.PI * this.radius ** 2;
  }
}
```

---

## ধাপ ৮: Generics

Generics দিয়ে টাইপ-safe এবং reusable কোড লেখা যায়।

```typescript
// একটি সাধারণ generic function
function identity<T>(value: T): T {
  return value;
}

identity<number>(5);
identity<string>("hello");

// Generic দিয়ে array wrapper
function getFirstElement<T>(arr: T[]): T {
  return arr[0];
}

getFirstElement<number>([1, 2, 3]); // 1

// Generic interface
interface ApiResponse<T> {
  success: boolean;
  data: T;
  message?: string;
}

const response: ApiResponse<User> = {
  success: true,
  data: { id: 1, name: "Rahim", email: "rahim@test.com" }
};

// Generic class
class Box<T> {
  private content: T;

  constructor(value: T) {
    this.content = value;
  }

  getContent(): T {
    return this.content;
  }
}

const numberBox = new Box<number>(100);
const stringBox = new Box<string>("Hello");

// Generic constraint (T কে নির্দিষ্ট কিছু হতে হবে)
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): void {
  console.log(item.length);
}

logLength("hello");     // ✅ string-এর length আছে
logLength([1, 2, 3]);   // ✅ array-এরও length আছে
```

---

## ধাপ ৯: Enum

```typescript
// Numeric enum
enum Role {
  Admin,     // 0
  Editor,    // 1
  Viewer     // 2
}

const userRole: Role = Role.Admin;

// String enum (বেশি readable, সাধারণত এটাই ভালো)
enum Status {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Banned = "BANNED"
}

function checkStatus(status: Status) {
  if (status === Status.Active) {
    console.log("ইউজার একটিভ");
  }
}
```

---

## ধাপ ১০: Utility Types (গুরুত্বপূর্ণ, প্রায়ই ব্যবহার হয়)

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

// Partial — সব প্রপার্টি অপশনাল করে দেয়
type UpdateUser = Partial<User>;
const update: UpdateUser = { name: "New Name" }; // বাকি ফিল্ড লাগবে না

// Pick — নির্দিষ্ট কিছু প্রপার্টি বেছে নেয়
type UserPreview = Pick<User, "id" | "name">;

// Omit — নির্দিষ্ট কিছু প্রপার্টি বাদ দেয়
type SafeUser = Omit<User, "password">;

// Required — সব প্রপার্টি required করে দেয়
type RequiredUser = Required<UpdateUser>;

// Readonly — সব প্রপার্টি readonly করে দেয়
type ImmutableUser = Readonly<User>;

// Record — key-value ম্যাপিং টাইপ তৈরি করে
type RolePermissions = Record<"admin" | "editor" | "viewer", string[]>;

const permissions: RolePermissions = {
  admin: ["create", "read", "update", "delete"],
  editor: ["create", "read", "update"],
  viewer: ["read"]
};
```

---

## ধাপ ১১: Type Narrowing (টাইপ নিশ্চিত করা)

```typescript
function printValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // এখানে TS জানে এটা string
  } else {
    console.log(value.toFixed(2)); // এখানে জানে এটা number
  }
}

// instanceof দিয়ে narrowing
class Cat {
  meow() { console.log("Meow"); }
}
class Dog {
  bark() { console.log("Bhow"); }
}

function makeSound(animal: Cat | Dog) {
  if (animal instanceof Cat) {
    animal.meow();
  } else {
    animal.bark();
  }
}

// in অপারেটর দিয়ে narrowing
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    animal.swim();
  } else {
    animal.fly();
  }
}
```

---

## ধাপ ১২: Async/Await ও Promise টাইপিং

```typescript
interface Post {
  id: number;
  title: string;
}

async function fetchPost(id: number): Promise<Post> {
  const response = await fetch(`https://api.example.com/posts/${id}`);
  const data: Post = await response.json();
  return data;
}

// Error handling সহ
async function safeFetch(id: number): Promise<Post | null> {
  try {
    const post = await fetchPost(id);
    return post;
  } catch (error) {
    console.error("Fetch failed:", error);
    return null;
  }
}
```

---

## ধাপ ১৩: বাস্তব উদাহরণ — একটি ছোট API সার্ভিস

তোমার Laravel API-এর সাথে কাজ করার মতো একটি বাস্তব উদাহরণ:

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  stock: number;
}

interface ApiResponse<T> {
  success: boolean;
  data: T;
  errors?: string[];
}

class ProductService {
  private baseUrl: string;

  constructor(baseUrl: string) {
    this.baseUrl = baseUrl;
  }

  async getAll(): Promise<Product[]> {
    const res = await fetch(`${this.baseUrl}/products`);
    const json: ApiResponse<Product[]> = await res.json();

    if (!json.success) {
      throw new Error(json.errors?.join(", ") ?? "Unknown error");
    }
    return json.data;
  }

  async getById(id: number): Promise<Product | null> {
    const res = await fetch(`${this.baseUrl}/products/${id}`);
    if (!res.ok) return null;

    const json: ApiResponse<Product> = await res.json();
    return json.data;
  }

  async create(product: Omit<Product, "id">): Promise<Product> {
    const res = await fetch(`${this.baseUrl}/products`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(product)
    });
    const json: ApiResponse<Product> = await res.json();
    return json.data;
  }
}

// ব্যবহার
const service = new ProductService("https://api.mysite.com");

service.getAll().then(products => {
  products.forEach(p => console.log(p.name, p.price));
});
```

---

## ধাপ ১৪: Next.js / Vue-তে TypeScript (সংক্ষেপে)

**Next.js-এ:**
```typescript
// pages/api/users.ts
import type { NextApiRequest, NextApiResponse } from "next";

type ResponseData = {
  message: string;
};

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>
) {
  res.status(200).json({ message: "Hello from TS API!" });
}
```

**Vue 3 (Composition API)-তে:**
```typescript
<script setup lang="ts">
import { ref } from "vue";

interface Todo {
  id: number;
  text: string;
  done: boolean;
}

const todos = ref<Todo[]>([]);

function addTodo(text: string): void {
  todos.value.push({ id: Date.now(), text, done: false });
}
</script>
```

---

## পরবর্তী ধাপে যা শেখা ভালো
- Mapped Types ও Conditional Types (অ্যাডভান্সড টাইপ ম্যানিপুলেশন)
- Decorators (Laravel-এর Annotation-এর মতো, NestJS-এ খুব common)
- Module ও Namespace
- Type Guards নিজে বানানো (`is` keyword দিয়ে)

কোনো নির্দিষ্ট অংশ নিয়ে আরও গভীরে যেতে চাইলে বলো — যেমন Generics আরও প্র্যাকটিস, বা Next.js/Vue প্রজেক্টে TypeScript সেটআপ করা নিয়ে বিস্তারিত।


---

# পার্ট ২: Generics — বিস্তারিত আলোচনা

Generics হলো TypeScript-এর সবচেয়ে শক্তিশালী ফিচারগুলোর একটি। এটা দিয়ে তুমি এমন কোড লিখতে পারো যা **একাধিক টাইপের সাথে কাজ করে, কিন্তু টাইপ-সেফটি ধরে রাখে**।

---

## ১. Generics কেন দরকার — সমস্যা থেকে বোঝা

ধরো তুমি একটা function চাও যেটা একটা array-এর প্রথম element রিটার্ন করবে।

**Generics ছাড়া (`any` দিয়ে) — সমস্যাযুক্ত পদ্ধতি:**
```typescript
function getFirst(arr: any[]): any {
  return arr[0];
}

const num = getFirst([1, 2, 3]);       // TS জানে না এটা number
const str = getFirst(["a", "b", "c"]); // TS জানে না এটা string

num.toUpperCase(); // ❌ Runtime এ crash করবে, কিন্তু TS কোনো error দেখাবে না!
```

`any` ব্যবহার করলে টাইপ চেকিং পুরোপুরি বন্ধ হয়ে যায় — এটাই মূল সমস্যা।

**Generics দিয়ে সমাধান:**
```typescript
function getFirst<T>(arr: T[]): T {
  return arr[0];
}

const num = getFirst([1, 2, 3]);       // TS জানে: T = number
const str = getFirst(["a", "b", "c"]); // TS জানে: T = string

num.toUpperCase(); // ✅ Compile-time এ error: number-এর toUpperCase নেই
```

এখানে `T` একটা **placeholder টাইপ** — যখন function কল করা হয়, তখন TS নিজে থেকেই বুঝে নেয় `T` আসলে কী টাইপ হবে (একে বলে **type inference**)।

---

## ২. একাধিক Type Parameter

```typescript
function pair<K, V>(key: K, value: V): [K, V] {
  return [key, value];
}

const p1 = pair("age", 25);        // [string, number]
const p2 = pair("active", true);   // [string, boolean]
```

## ৩. Generic Constraints (`extends`)

কখনো কখনো `T` কে সম্পূর্ণ free রাখতে চাও না — নির্দিষ্ট শর্ত মানতে হবে।

```typescript
interface HasId {
  id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
  return items.find(item => item.id === id);
}

// এটা কাজ করবে কারণ id প্রপার্টি আছে
findById([{ id: 1, name: "Rahim" }], 1);

// findById([{ name: "Rahim" }], 1); // ❌ Error: id প্রপার্টি নেই
```

## ৪. Default Generic Type

```typescript
interface ApiResponse<T = unknown> {
  success: boolean;
  data: T;
}

const res1: ApiResponse = { success: true, data: "কিছু একটা" };       // T = unknown
const res2: ApiResponse<number> = { success: true, data: 100 };       // T = number
```

## ৫. `keyof` দিয়ে Generic — Object প্রপার্টি সেফলি অ্যাক্সেস করা

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { id: 1, name: "Karim", email: "karim@test.com" };

getProperty(user, "name");   // ✅ string রিটার্ন করবে
// getProperty(user, "age"); // ❌ Error: "age" user-এর কোনো key না
```

এখন — এই কনসেপ্টগুলো বাস্তব প্রজেক্টে কীভাবে ব্যবহার হয়, তার ৩টি সম্পূর্ণ উদাহরণ দেখি।

---

## প্রজেক্ট উদাহরণ ১: Generic API Client (Fetch Wrapper)

যেকোনো REST API-এর সাথে কাজ করার জন্য একটা reusable, টাইপ-সেফ API client।

```typescript
// api-client.ts

interface ApiSuccessResponse<T> {
  success: true;
  data: T;
}

interface ApiErrorResponse {
  success: false;
  message: string;
  statusCode: number;
}

type ApiResult<T> = ApiSuccessResponse<T> | ApiErrorResponse;

class ApiClient {
  constructor(private baseUrl: string) {}

  async get<T>(endpoint: string): Promise<ApiResult<T>> {
    try {
      const res = await fetch(`${this.baseUrl}${endpoint}`);
      if (!res.ok) {
        return { success: false, message: res.statusText, statusCode: res.status };
      }
      const data: T = await res.json();
      return { success: true, data };
    } catch (err) {
      return { success: false, message: "Network error", statusCode: 500 };
    }
  }

  async post<TBody, TResponse>(endpoint: string, body: TBody): Promise<ApiResult<TResponse>> {
    try {
      const res = await fetch(`${this.baseUrl}${endpoint}`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(body)
      });
      if (!res.ok) {
        return { success: false, message: res.statusText, statusCode: res.status };
      }
      const data: TResponse = await res.json();
      return { success: true, data };
    } catch (err) {
      return { success: false, message: "Network error", statusCode: 500 };
    }
  }
}

// ---- ব্যবহার ----

interface User {
  id: number;
  name: string;
  email: string;
}

interface CreateUserPayload {
  name: string;
  email: string;
  password: string;
}

const client = new ApiClient("https://api.mysite.com");

async function loadUsers() {
  const result = await client.get<User[]>("/users");

  if (result.success) {
    // এখানে TS জানে result.data হলো User[]
    result.data.forEach(u => console.log(u.name));
  } else {
    console.error(result.message);
  }
}

async function registerUser() {
  const payload: CreateUserPayload = {
    name: "Rahim",
    email: "rahim@test.com",
    password: "secret123"
  };

  // এখানে TBody = CreateUserPayload, TResponse = User — দুটোই আলাদা টাইপ
  const result = await client.post<CreateUserPayload, User>("/users", payload);

  if (result.success) {
    console.log(`নতুন ইউজার তৈরি হয়েছে: ${result.data.id}`);
  }
}
```

**এখানে Generics কী সমাধান করছে:** একটাই `ApiClient` class দিয়ে তুমি `User`, `Product`, `Order` — যেকোনো ধরনের ডেটার জন্য কল করতে পারছো, প্রতিবার আলাদা class না লিখেই, আর প্রতিবারই সঠিক টাইপ পাচ্ছো।

---

## প্রজেক্ট উদাহরণ ২: Generic Repository Pattern (In-Memory Data Store)

Laravel-এর Eloquent Model-এর মতো একটা concept — একটা generic "Repository" যেটা যেকোনো এন্টিটির জন্য CRUD অপারেশন হ্যান্ডেল করে।

```typescript
// repository.ts

interface Entity {
  id: number;
}

class Repository<T extends Entity> {
  private items: T[] = [];
  private nextId = 1;

  create(data: Omit<T, "id">): T {
    const newItem = { ...data, id: this.nextId++ } as T;
    this.items.push(newItem);
    return newItem;
  }

  findAll(): T[] {
    return this.items;
  }

  findById(id: number): T | undefined {
    return this.items.find(item => item.id === id);
  }

  update(id: number, changes: Partial<Omit<T, "id">>): T | undefined {
    const item = this.findById(id);
    if (!item) return undefined;

    Object.assign(item, changes);
    return item;
  }

  delete(id: number): boolean {
    const index = this.items.findIndex(item => item.id === id);
    if (index === -1) return false;

    this.items.splice(index, 1);
    return true;
  }
}

// ---- ব্যবহার: দুইটা ভিন্ন এন্টিটির জন্য একই Repository ক্লাস ----

interface Product extends Entity {
  name: string;
  price: number;
  stock: number;
}

interface Customer extends Entity {
  fullName: string;
  phone: string;
}

const productRepo = new Repository<Product>();
const customerRepo = new Repository<Customer>();

const laptop = productRepo.create({ name: "Laptop", price: 55000, stock: 10 });
const mouse = productRepo.create({ name: "Mouse", price: 500, stock: 100 });

productRepo.update(laptop.id, { stock: 8 });

const rahim = customerRepo.create({ fullName: "Rahim Uddin", phone: "01700000000" });

console.log(productRepo.findAll());   // Product[] টাইপ
console.log(customerRepo.findById(rahim.id)); // Customer | undefined
```

**এখানে Generics কী সমাধান করছে:** `Product` আর `Customer` দুটো সম্পূর্ণ ভিন্ন ধরনের ডেটা, কিন্তু একই `Repository<T>` ক্লাস দিয়ে দুটোর CRUD লজিক লেখা হয়েছে — কোড ডুপ্লিকেশন নেই, আর টাইপ-সেফটিও ঠিক আছে।

---

## প্রজেক্ট উদাহরণ ৩: Generic Form Validator (React/Vue ফর্মের জন্য উপযোগী)

একটা টাইপ-সেফ ফর্ম ভ্যালিডেশন সিস্টেম, যেকোনো shape-এর ফর্ম ডেটার জন্য কাজ করবে।

```typescript
// validator.ts

type ValidationRule<T> = (value: T) => string | null; // null মানে ভ্যালিড, string মানে error message

type ValidationSchema<T> = {
  [K in keyof T]?: ValidationRule<T[K]>[];
};

type ValidationErrors<T> = {
  [K in keyof T]?: string[];
};

class FormValidator<T extends Record<string, unknown>> {
  constructor(private schema: ValidationSchema<T>) {}

  validate(data: T): { valid: boolean; errors: ValidationErrors<T> } {
    const errors: ValidationErrors<T> = {};
    let valid = true;

    for (const key in this.schema) {
      const rules = this.schema[key];
      if (!rules) continue;

      const fieldErrors: string[] = [];
      for (const rule of rules) {
        const error = rule(data[key]);
        if (error) fieldErrors.push(error);
      }

      if (fieldErrors.length > 0) {
        errors[key] = fieldErrors;
        valid = false;
      }
    }

    return { valid, errors };
  }
}

// ---- সাধারণ কিছু reusable rule ----

const required = <T>(value: T): string | null =>
  value === "" || value === null || value === undefined ? "এই ফিল্ডটি আবশ্যক" : null;

const minLength = (min: number) => (value: string): string | null =>
  value.length < min ? `কমপক্ষে ${min} অক্ষর হতে হবে` : null;

const isEmail = (value: string): string | null =>
  /\S+@\S+\.\S+/.test(value) ? null : "সঠিক ইমেইল দিন";

const minValue = (min: number) => (value: number): string | null =>
  value < min ? `মান কমপক্ষে ${min} হতে হবে` : null;

// ---- ব্যবহার: রেজিস্ট্রেশন ফর্ম ----

interface RegisterForm {
  name: string;
  email: string;
  password: string;
  age: number;
}

const registerValidator = new FormValidator<RegisterForm>({
  name: [required, minLength(3)],
  email: [required, isEmail],
  password: [required, minLength(6)],
  age: [minValue(18)]
});

const formData: RegisterForm = {
  name: "Ra",
  email: "invalid-email",
  password: "123",
  age: 16
};

const result = registerValidator.validate(formData);

console.log(result.valid);   // false
console.log(result.errors);
/*
{
  name: ["কমপক্ষে 3 অক্ষর হতে হবে"],
  email: ["সঠিক ইমেইল দিন"],
  password: ["কমপক্ষে 6 অক্ষর হতে হবে"],
  age: ["মান কমপক্ষে 18 হতে হবে"]
}
*/
```

**এখানে Generics কী সমাধান করছে:** `ValidationSchema<T>` একটা **mapped type** ব্যবহার করছে (`[K in keyof T]`) যাতে schema-টা ঠিক তোমার form interface-এর সাথে মিলে যায়। তুমি যদি `RegisterForm`-এ ভুল field নাম দাও (যেমন `emial`), TS সাথে সাথেই error দেখাবে। একই `FormValidator<T>` ক্লাস দিয়ে লগইন ফর্ম, প্রোডাক্ট ফর্ম — যেকোনো ফর্মের জন্য ব্যবহার করা যাবে।

---

## সংক্ষেপে — এই ৩টি উদাহরণ থেকে যা শেখার

| প্রজেক্ট | মূল Generics কনসেপ্ট |
|---|---|
| API Client | Multiple type parameters (`TBody`, `TResponse`), Union return type |
| Repository Pattern | Generic constraint (`T extends Entity`), `Omit` ও `Partial` এর সাথে সমন্বয় |
| Form Validator | Mapped types (`[K in keyof T]`), Higher-order generic functions |

চাইলে এই তিনটার যেকোনো একটা আরও বড় করে (যেমন Repository-তে LocalStorage/API integration যোগ করে, বা Validator-এ async validation যোগ করে) দেখাতে পারি।


---

# পার্ট ৩: Generics প্রজেক্ট — Extended ভার্সন

আগের ৩টি উদাহরণকে আরও বাস্তবসম্মত ও প্রোডাকশন-রেডি করে দেখানো হলো।

---

## ১. Extended API Client — Retry, Caching, Auth Headers, Interceptor

আগেরটায় শুধু GET/POST ছিল। এখন যোগ হলো: PUT/DELETE, automatic retry, response caching, auth token, এবং request/response interceptor।

```typescript
// api-client.ts

interface ApiSuccessResponse<T> {
  success: true;
  data: T;
}

interface ApiErrorResponse {
  success: false;
  message: string;
  statusCode: number;
}

type ApiResult<T> = ApiSuccessResponse<T> | ApiErrorResponse;

interface RequestOptions {
  retries?: number;
  useCache?: boolean;
  cacheTtlMs?: number;
}

type RequestInterceptor = (config: RequestInit) => RequestInit;
type ResponseInterceptor = <T>(result: ApiResult<T>) => ApiResult<T>;

class ApiClient {
  private authToken: string | null = null;
  private cache = new Map<string, { data: unknown; expiresAt: number }>();
  private requestInterceptors: RequestInterceptor[] = [];

  constructor(private baseUrl: string) {}

  setAuthToken(token: string): void {
    this.authToken = token;
  }

  addRequestInterceptor(interceptor: RequestInterceptor): void {
    this.requestInterceptors.push(interceptor);
  }

  private buildConfig(config: RequestInit = {}): RequestInit {
    let finalConfig: RequestInit = {
      ...config,
      headers: {
        "Content-Type": "application/json",
        ...(this.authToken ? { Authorization: `Bearer ${this.authToken}` } : {}),
        ...config.headers
      }
    };

    for (const interceptor of this.requestInterceptors) {
      finalConfig = interceptor(finalConfig);
    }
    return finalConfig;
  }

  private async requestWithRetry<T>(
    url: string,
    config: RequestInit,
    retries: number
  ): Promise<ApiResult<T>> {
    try {
      const res = await fetch(url, config);
      if (!res.ok) {
        if (retries > 0 && res.status >= 500) {
          return this.requestWithRetry<T>(url, config, retries - 1);
        }
        return { success: false, message: res.statusText, statusCode: res.status };
      }
      const data: T = await res.json();
      return { success: true, data };
    } catch (err) {
      if (retries > 0) {
        return this.requestWithRetry<T>(url, config, retries - 1);
      }
      return { success: false, message: "Network error", statusCode: 500 };
    }
  }

  async get<T>(endpoint: string, options: RequestOptions = {}): Promise<ApiResult<T>> {
    const { retries = 1, useCache = false, cacheTtlMs = 60_000 } = options;
    const cacheKey = `GET:${endpoint}`;

    if (useCache) {
      const cached = this.cache.get(cacheKey);
      if (cached && cached.expiresAt > Date.now()) {
        return { success: true, data: cached.data as T };
      }
    }

    const config = this.buildConfig({ method: "GET" });
    const result = await this.requestWithRetry<T>(`${this.baseUrl}${endpoint}`, config, retries);

    if (useCache && result.success) {
      this.cache.set(cacheKey, { data: result.data, expiresAt: Date.now() + cacheTtlMs });
    }
    return result;
  }

  async post<TBody, TResponse>(
    endpoint: string,
    body: TBody,
    options: RequestOptions = {}
  ): Promise<ApiResult<TResponse>> {
    const config = this.buildConfig({ method: "POST", body: JSON.stringify(body) });
    return this.requestWithRetry<TResponse>(`${this.baseUrl}${endpoint}`, config, options.retries ?? 0);
  }

  async put<TBody, TResponse>(
    endpoint: string,
    body: TBody,
    options: RequestOptions = {}
  ): Promise<ApiResult<TResponse>> {
    const config = this.buildConfig({ method: "PUT", body: JSON.stringify(body) });
    return this.requestWithRetry<TResponse>(`${this.baseUrl}${endpoint}`, config, options.retries ?? 0);
  }

  async delete<TResponse>(endpoint: string, options: RequestOptions = {}): Promise<ApiResult<TResponse>> {
    const config = this.buildConfig({ method: "DELETE" });
    return this.requestWithRetry<TResponse>(`${this.baseUrl}${endpoint}`, config, options.retries ?? 0);
  }
}

// ---- ব্যবহার ----

interface User {
  id: number;
  name: string;
  email: string;
}

const client = new ApiClient("https://api.mysite.com");
client.setAuthToken("jwt-token-here");

// Request log করার জন্য interceptor
client.addRequestInterceptor(config => {
  console.log(`Request পাঠানো হচ্ছে:`, config.method);
  return config;
});

async function demo() {
  // Cache সহ GET, 3 বার পর্যন্ত retry
  const users = await client.get<User[]>("/users", { useCache: true, retries: 3 });

  if (users.success) {
    console.log(users.data);
  }

  const updated = await client.put<Partial<User>, User>("/users/1", { name: "নতুন নাম" });
  const deleted = await client.delete<{ deleted: boolean }>("/users/1");
}
```

**নতুন কী যোগ হলো:** Auth token স্বয়ংক্রিয়ভাবে header-এ বসে, network fail হলে নিজে থেকেই retry করে, বারবার একই GET call করলে cache থেকে ডেটা দেয়, আর interceptor দিয়ে প্রতিটা request-এর আগে custom logic (logging, header modify) চালানো যায় — সবকিছুই generic টাইপ ধরে রেখে।

---

## ২. Extended Repository — Query Filtering, Sorting, Pagination, Persistence

আগেরটায় শুধু in-memory CRUD ছিল। এখন যোগ হলো: filter/sort/pagination, এবং localStorage-এ persist করার সুবিধা (browser environment-এ, যেমন Vue/Next.js প্রজেক্টে)।

```typescript
// repository.ts

interface Entity {
  id: number;
}

interface PaginationOptions {
  page: number;
  perPage: number;
}

interface PaginatedResult<T> {
  items: T[];
  total: number;
  page: number;
  totalPages: number;
}

type FilterFn<T> = (item: T) => boolean;
type SortFn<T> = (a: T, b: T) => number;

class Repository<T extends Entity> {
  private items: T[] = [];
  private nextId = 1;

  constructor(private storageKey?: string) {
    if (this.storageKey) this.loadFromStorage();
  }

  private loadFromStorage(): void {
    if (typeof localStorage === "undefined" || !this.storageKey) return;
    const raw = localStorage.getItem(this.storageKey);
    if (raw) {
      const parsed = JSON.parse(raw) as { items: T[]; nextId: number };
      this.items = parsed.items;
      this.nextId = parsed.nextId;
    }
  }

  private saveToStorage(): void {
    if (typeof localStorage === "undefined" || !this.storageKey) return;
    localStorage.setItem(this.storageKey, JSON.stringify({ items: this.items, nextId: this.nextId }));
  }

  create(data: Omit<T, "id">): T {
    const newItem = { ...data, id: this.nextId++ } as T;
    this.items.push(newItem);
    this.saveToStorage();
    return newItem;
  }

  findAll(filter?: FilterFn<T>, sort?: SortFn<T>): T[] {
    let result = filter ? this.items.filter(filter) : [...this.items];
    if (sort) result = result.sort(sort);
    return result;
  }

  findPaginated(
    { page, perPage }: PaginationOptions,
    filter?: FilterFn<T>,
    sort?: SortFn<T>
  ): PaginatedResult<T> {
    const filtered = this.findAll(filter, sort);
    const start = (page - 1) * perPage;
    const items = filtered.slice(start, start + perPage);

    return {
      items,
      total: filtered.length,
      page,
      totalPages: Math.ceil(filtered.length / perPage)
    };
  }

  findById(id: number): T | undefined {
    return this.items.find(item => item.id === id);
  }

  update(id: number, changes: Partial<Omit<T, "id">>): T | undefined {
    const item = this.findById(id);
    if (!item) return undefined;

    Object.assign(item, changes);
    this.saveToStorage();
    return item;
  }

  delete(id: number): boolean {
    const index = this.items.findIndex(item => item.id === id);
    if (index === -1) return false;

    this.items.splice(index, 1);
    this.saveToStorage();
    return true;
  }
}

// ---- ব্যবহার ----

interface Product extends Entity {
  name: string;
  price: number;
  category: string;
  stock: number;
}

// "products" নামে localStorage-এ সেভ হবে
const productRepo = new Repository<Product>("products");

productRepo.create({ name: "Laptop", price: 55000, category: "Electronics", stock: 10 });
productRepo.create({ name: "Chair", price: 3000, category: "Furniture", stock: 25 });
productRepo.create({ name: "Mouse", price: 500, category: "Electronics", stock: 100 });

// শুধু Electronics ক্যাটাগরি, দাম অনুযায়ী sort
const electronics = productRepo.findAll(
  p => p.category === "Electronics",
  (a, b) => a.price - b.price
);

// Pagination সহ
const page1 = productRepo.findPaginated(
  { page: 1, perPage: 2 },
  p => p.stock > 0
);

console.log(page1);
// { items: [...দুইটা প্রোডাক্ট...], total: 3, page: 1, totalPages: 2 }
```

**নতুন কী যোগ হলো:** এখন filter (`p => p.category === "Electronics"`), sort, আর pagination সাপোর্ট করে — যা তোমার Laravel API-এর `paginate()`-এর মতোই কাজ করে। আর `storageKey` দিলে ডেটা browser-এর localStorage-এ persist থাকে, পেজ রিফ্রেশ করলেও হারায় না।

---

## ৩. Extended Form Validator — Async Validation ও Cross-Field Rules

আগেরটায় শুধু sync validation ছিল। এখন যোগ হলো: async validation (যেমন backend-এ email duplicate চেক করা) এবং cross-field validation (যেমন password ও confirm-password মিলছে কিনা)।

```typescript
// validator.ts

type SyncRule<T> = (value: T, allValues: Record<string, unknown>) => string | null;
type AsyncRule<T> = (value: T, allValues: Record<string, unknown>) => Promise<string | null>;
type ValidationRule<T> = SyncRule<T> | AsyncRule<T>;

type ValidationSchema<T> = {
  [K in keyof T]?: ValidationRule<T[K]>[];
};

type ValidationErrors<T> = {
  [K in keyof T]?: string[];
};

class FormValidator<T extends Record<string, unknown>> {
  constructor(private schema: ValidationSchema<T>) {}

  async validate(data: T): Promise<{ valid: boolean; errors: ValidationErrors<T> }> {
    const errors: ValidationErrors<T> = {};
    let valid = true;

    for (const key in this.schema) {
      const rules = this.schema[key];
      if (!rules) continue;

      const fieldErrors: string[] = [];

      for (const rule of rules) {
        // Sync ও async দুই ধরনের rule-ই await করা যায়, কারণ Promise.resolve() sync ভ্যালুকেও wrap করে
        const error = await Promise.resolve(rule(data[key], data));
        if (error) fieldErrors.push(error);
      }

      if (fieldErrors.length > 0) {
        errors[key] = fieldErrors;
        valid = false;
      }
    }

    return { valid, errors };
  }
}

// ---- Reusable rules ----

const required = <T>(value: T): string | null =>
  value === "" || value === null || value === undefined ? "এই ফিল্ডটি আবশ্যক" : null;

const minLength = (min: number) => (value: string): string | null =>
  value.length < min ? `কমপক্ষে ${min} অক্ষর হতে হবে` : null;

const isEmail = (value: string): string | null =>
  /\S+@\S+\.\S+/.test(value) ? null : "সঠিক ইমেইল দিন";

// Cross-field rule: confirmPassword, password-এর সাথে মিলছে কিনা
const matchesField = <T>(fieldName: string, label: string) =>
  (value: T, allValues: Record<string, unknown>): string | null =>
    value !== allValues[fieldName] ? `${label} মিলছে না` : null;

// Async rule: সার্ভারে গিয়ে email duplicate চেক করা (simulate করা হলো)
async function checkEmailAvailable(email: string): Promise<boolean> {
  await new Promise(resolve => setTimeout(resolve, 300)); // নেটওয়ার্ক দেরি simulate
  const takenEmails = ["rahim@test.com", "karim@test.com"];
  return !takenEmails.includes(email);
}

const emailNotTaken: AsyncRule<string> = async (value) => {
  const available = await checkEmailAvailable(value);
  return available ? null : "এই ইমেইল দিয়ে ইতিমধ্যে অ্যাকাউন্ট আছে";
};

// ---- ব্যবহার ----

interface RegisterForm {
  name: string;
  email: string;
  password: string;
  confirmPassword: string;
}

const registerValidator = new FormValidator<RegisterForm>({
  name: [required, minLength(3)],
  email: [required, isEmail, emailNotTaken],
  password: [required, minLength(6)],
  confirmPassword: [required, matchesField("password", "Confirm Password")]
});

async function handleRegister() {
  const formData: RegisterForm = {
    name: "Rahim",
    email: "rahim@test.com", // ইতিমধ্যেই ব্যবহৃত
    password: "secret123",
    confirmPassword: "secret124" // মিলছে না
  };

  const result = await registerValidator.validate(formData);

  console.log(result.valid); // false
  console.log(result.errors);
  /*
  {
    email: ["এই ইমেইল দিয়ে ইতিমধ্যে অ্যাকাউন্ট আছে"],
    confirmPassword: ["Confirm Password মিলছে না"]
  }
  */
}

handleRegister();
```

**নতুন কী যোগ হলো:** `emailNotTaken`-এর মতো rule এখন async হতে পারে (সার্ভারে গিয়ে ডেটা চেক করে), আর `matchesField`-এর মতো rule অন্য ফিল্ডের ভ্যালুও দেখতে পারে (cross-field validation)। `Promise.resolve()` ব্যবহার করে sync আর async rule দুটোকেই একই লজিকে হ্যান্ডেল করা হয়েছে — এটাও একটা common generics/TypeScript প্যাটার্ন।

---

## সংক্ষেপে — নতুন কী শেখা গেল

| প্রজেক্ট | নতুন যোগ হওয়া ফিচার | মূল TypeScript কৌশল |
|---|---|---|
| API Client | Retry, Cache, Auth, Interceptor | Optional config object, function type হিসেবে interceptor |
| Repository | Filter/Sort/Pagination, localStorage persistence | Function-as-parameter generics (`FilterFn<T>`, `SortFn<T>`) |
| Form Validator | Async validation, Cross-field rules | Union of sync/async function types, `Promise.resolve()` প্যাটার্ন |

এগুলোর যেকোনো একটা নিয়ে আরও গভীরে যেতে চাইলে — যেমন Repository-কে সত্যিকারের Laravel API-এর সাথে কানেক্ট করা, বা Validator-কে Vue/React ফর্মের সাথে সরাসরি ইন্টিগ্রেট করা দেখাতে পারি।
