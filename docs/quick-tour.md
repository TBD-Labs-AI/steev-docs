# Quick Tour


## Getting Started

After you have installed Steev, you can start using it by running the following command:

```bash
steev run train.py
```

If you have previously used additional arguments to edit parameters, you can use the same arguments to run Steev.

```bash
steev run train.py --kwargs learning-rate=0.001 batch-size=32
# Or, steev run train.py --kwargs lr=0.001 bs=32
```

### What Happens Next?

When you run this command, Steev will:

1. Review Your Code
    - Analyze architecture & dataset
    - Check configurations
    - Suggest improvements
  
2. Monitor Training and Proactively Respond
    - Track metrics
    - Detect anomalies
    - Manage resources

3. Optimize & Deliver
    - Fine-tune parameters
    - Select best weights
    - Generate reports

## Next Steps

Ready to dive deeper? Here's what you can do next:

1. 📚 Read our [Installation Guide](installation.md)
2. 💡 Try the [Tutorials](../tutorials/run-with-script.md)
3. 👥 Join our [Discord Community](https://discord.gg/steev)
4. 🔧 Explore advanced features:
   - Custom optimization strategies
   - Distributed training
   - Framework integrations 