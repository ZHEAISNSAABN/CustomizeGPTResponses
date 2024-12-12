## How to Use the Datasets for **Create GPT**

This guide explains how to configure and customize OpenAI’s **Create GPT** feature to reflect your team’s coding style, architectural preferences, and best practices—particularly for .NET development. By following these steps, you’ll integrate your datasets and coding guidelines into a custom GPT without needing to fine-tune the model directly.

---

### **Step 1: Access the "Create GPT" Feature in OpenAI**
1. **Log In to the OpenAI Dashboard**: Go to [OpenAI’s platform](https://platform.openai.com/) and sign in with your team’s account. Make sure you have the appropriate permissions to create and manage GPT instances.
2. **Navigate to the "Create GPT" Section**: In the dashboard sidebar, look for the **“Create GPT”** option. This is where you’ll configure custom instructions, messages, and examples.
3. **Start Creating Your GPT**: Click **“Create New GPT”** and provide:
   - A **Name**: For example, `".NET Junior Developer Assistant"` or any name that indicates its purpose.
   - A **Description/Purpose**: Briefly describe what this GPT is for, such as “A helper that provides .NET code samples, enforces certain architectural patterns, and suggests improvements based on Clean Architecture principles.”

**Why This Matters**: Defining the name and purpose upfront sets a clear intent for how the model should behave and allows your team to easily identify and manage multiple GPT instances if you create more in the future.

---

### **Step 2: Configure the System Message**
OpenAI’s **Create GPT** feature uses a **System Message** to guide the overall behavior of the model. This is where you set the “ground rules” the GPT should follow. Think of the System Message as the model’s highest-level instructions—anything you specify here takes precedence over user prompts.

**What to Include in the System Message**:  
- **Coding Conventions**: Outline coding principles and patterns you expect the GPT to follow.
- **Architectural Guidance**: Include details on your preferred architecture (like Clean Architecture layers).
- **Do’s and Don’ts**: Specify what it should avoid doing (e.g., certain return types like `IQueryable`).
- **Formatting and Explanation Details**: Instruct the GPT to format code cleanly, provide explanations upon request, and offer alternative ideas.

**Example System Message** (tailored for .NET):
```plaintext
You are a highly skilled .NET junior developer assistant trained to follow these coding principles:

1. Use dependency injection in all service classes.
2. Avoid returning `IQueryable` from repositories; use clean, strongly-typed collections and abstractions instead.
3. Implement and encourage the repository and specification patterns.
4. Follow Clean Architecture principles:
   - Core Layer: Contains domain entities and business rules.
   - Application Layer: Defines interfaces, use cases, and application-specific logic.
   - Infrastructure Layer: Provides EF Core implementations, logging, and other integrations.
5. Write code that is clean, maintainable, and optimized for performance.
6. When asked, provide alternative suggestions, refactoring tips, or improvements.

When responding to user prompts:
- Always format code properly for readability.
- Explain your decisions if the user requests more detail.
- Adhere strictly to the outlined best practices.
```

**Why This Matters**: The System Message sets the model’s default “personality” and ensures that every answer aligns with your coding philosophy and standards.

---

### **Step 3: Add Sample Data and Examples**
After setting the System Message, the next critical step is to add **Examples**. Examples demonstrate to the model how you expect it to respond to certain types of requests. By providing well-structured examples, you shape the model’s tendencies and ensure consistent outputs aligned with your team’s standards.

1. **Identify Common Use Cases**: Think of typical requests your developers will make, such as:
   - Creating repositories, services, and entities.
   - Implementing middleware.
   - Explaining architectural concepts.
   - Suggesting alternative patterns or improvements.
   
2. **Add Each Example**: Each example consists of an **Input** (user prompt) and a corresponding **Output** (the expected model completion).

**Example 1: Repository Pattern**
- **Input**:
  ```plaintext
  Write a repository for managing products in .NET.
  ```
- **Output**:
  ```csharp
  public class ProductRepository : IProductRepository {
      private readonly ApplicationDbContext _context;

      public ProductRepository(ApplicationDbContext context) {
          _context = context;
      }

      public async Task<Product> GetByIdAsync(int id) {
          return await _context.Products.FirstOrDefaultAsync(p => p.Id == id);
      }

      public async Task<IReadOnlyList<Product>> GetAllAsync() {
          return await _context.Products.ToListAsync();
      }

      public async Task AddAsync(Product product) {
          await _context.Products.AddAsync(product);
          await _context.SaveChangesAsync();
      }

      public async Task UpdateAsync(Product product) {
          _context.Products.Update(product);
          await _context.SaveChangesAsync();
      }

      public async Task DeleteAsync(Product product) {
          _context.Products.Remove(product);
          await _context.SaveChangesAsync();
      }
  }
  ```

**Example 2: Middleware**
- **Input**:
  ```plaintext
  Create middleware for logging HTTP requests in ASP.NET Core.
  ```
- **Output**:
  ```csharp
  public class RequestLoggingMiddleware {
      private readonly RequestDelegate _next;

      public RequestLoggingMiddleware(RequestDelegate next) {
          _next = next;
      }

      public async Task InvokeAsync(HttpContext context) {
          Console.WriteLine($"Request: {context.Request.Method} {context.Request.Path}");
          await _next(context);
      }
  }
  ```

**Example 3: Clean Architecture Explanation**
- **Input**:
  ```plaintext
  Explain the structure of a Clean Architecture .NET project.
  ```
- **Output**:
  ```plaintext
  A Clean Architecture project in .NET is divided into these layers:
  1. Core Layer: Domain entities, value objects, and business rules.
  2. Application Layer: Interfaces, use cases, and application logic.
  3. Infrastructure Layer: Database interactions (e.g., EF Core), logging, external services.
  4. API Layer: Entry points (controllers or endpoints) for clients.

  This layered approach ensures scalability, maintainability, and testability.
  ```

**Example 4: Suggesting Alternatives**
- **Input**:
  ```plaintext
  What’s an alternative to the repository pattern for EF Core?
  ```
- **Output**:
  ```plaintext
  An alternative is the Query Service pattern. Instead of traditional repositories, create specialized services that encapsulate common queries directly against the DbContext. This can reduce abstraction layers and more fully leverage EF Core's querying capabilities.
  ```

**Why This Matters**: Examples serve as a training-by-demonstration mechanism. They inform the model about the style, tone, and level of detail you want. Including a variety of examples ensures the model is prepared for different scenarios.

---

### **Step 4: Test Your Custom GPT**
1. **Save Your Configuration**: Once you’ve set the System Message and examples, save your GPT configuration.
2. **Run a Few Prompts**: Test your custom GPT by asking it questions or requesting code snippets.
   - Check if the responses match the formatting, architectural guidance, and coding style you outlined.
   - Ask it to provide explanations or alternative suggestions to confirm it follows all instructions.

3. **Refine As Needed**: If you notice responses that don’t align with your standards, return to the System Message or examples and adjust accordingly. You can add new examples, refine wording, or enforce stricter rules in the System Message.

**Why This Matters**: Iterative testing ensures that your GPT is meeting your expectations. Early testing helps you catch and correct issues before wider team usage.

---

### **Step 5: Iterate and Improve Over Time**
Your custom GPT is not static—think of it as a living document of coding practices.

1. **Collect Feedback**: Encourage your team to use the GPT and note where it excels or falls short.
2. **Update the Dataset**: Add new examples that address gaps or misunderstandings. For instance, if you find the GPT doesn’t follow a certain naming convention, add an example showing the correct pattern.
3. **Enhance the System Message**: Clarify instructions whenever the model seems confused or deviates from expected behavior.

**Why This Matters**: Continual improvement will help your GPT become an increasingly reliable assistant that reflects evolving best practices and new insights from your development team.

---

### **Key Benefits of Using "Create GPT"**
1. **No Additional Training Required**: You don’t have to create a full fine-tune training process. “Create GPT” allows configuration on top of existing models.
2. **Immediate Updates**: Quickly refine the GPT’s behavior by editing System Messages and examples, without waiting for lengthy training cycles.
3. **Data Collection for Future Fine-Tuning**: As you improve the GPT and gather a collection of well-crafted examples, you’ll build a strong dataset that can be used for future fine-tuning if desired.

---

**Next Steps**:  
- Share this guide with your colleagues so they understand how to set up their own GPT instances aligned with team standards.
- Start small by adding a few examples and refine as you go.
- Consider creating a shared library of examples and System Messages that everyone can contribute to.

If you need assistance with the initial configuration or want to review the first set of examples, feel free to ask for help.
