# Grad School Notes

## **Supervised Learning** 
Function approximation from examples  
**Goal**: Learn a function \( y = f(x) \) from labeled data  
**Given**: Pairs of inputs and outputs \((x, y)\)  
**Learn**: A function \( f(x) \) that maps inputs to outputs and generalizes to unseen \( x \)  
**Examples**: classification, regression

## **Unsupervised Learning**  
Clustering, description, compression  
**Goal**: Learn data structure and description from input data alone  
**Given**: Inputs \( x \)  
**Learn**: A function \( f(x) \) that captures structure, clusters, or reduces dimensionality  

## **Reinforcement Learning**  
Trial and error with feedback  
**Goal**: Learn how to make decisions  
**Given**: States \( x \), actions \( y \), and delayed rewards \( z \)  
**Learn**: A function or policy \( y = f(x) \) that maps states to actions to maximize reward  
**Examples**: games, robotics, recommendation systems
___
## **Topics so far**

- [Machine Learning](ml/index.md)  
- [Natural Language Processing](nlp/index.md)  
- [Deep Learning](dl/index.md)  
- [AI for Robotics](robotics/index.md)  
- [Reinforced Learning](rl/index.md)  

---
## **Future Projects**

When I finish grad school

### **Junk Yard**  

**Auto parts database**: Add all relevant auto parts in the world to database capturing key info like, photo, size, weight, id numbers.  
- How do we define relevant? We need to find most popular reused parts. Based on car model? Car date? Parts used across models?  
- What about rare parts? Should we always keep since the world is running out of them?  

**Computer Vision**: Input live photo or video into CV component connected to database to access entry for that part  
- Stocks: Gather fresh data to plug into Stock Predictor  

### **Test Bed**

Set up test env and dataset(s) so I can easily test improvements (of models, loss functions, optimizers, privacy, data transforms etc.). Create bench marking in a variety of metrics. Save model params and optimizer. Saving optimizer allows us to continue training where we left off.

Note: Keep notes on all steps in research spike.