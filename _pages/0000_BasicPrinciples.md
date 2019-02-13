
### <a name="av0000"></a>The Principle of Least Surprise (AV0000) ![](/assets/images/1.png)

You should choose a solution that everyone can understand, and that keeps them on the right track.

Keep in mind that your not alone, or that you will come back to that code in a few years.

### <a name="av0001"></a>Keep It Simple Stupid (a.k.a. KISS) (AV0001) ![](/assets/images/1.png)
The simplest solution is more than sufficient.

But not only, it is also the easiest to understand for the others.

Noticed that most of the time, if the solution is not simple, it mean that the problems are not well
understood, or that you don't have a clear idea of the problem.

Trying to have a clear and simple solution is only possible if the solution is simple, well structured, organized and documented.

### <a name="av0002"></a>You Ain’t Gonna Need It (a.k.a. YAGNI)(AV0002) ![](/assets/images/1.png)

Create a solution for the problem at hand, not for the ones you think may happen later on. Can you predict the future?

### <a name="av0003"></a>Don’t Repeat Yourself (a.k.a. DRY)(AV0003) ![](/assets/images/1.png)
Avoid duplication within a component, a source control repository or a bounded context, without forgetting the Rule of Three heuristic.

### <a name="av0004"></a>The rule of 30(AV0004) ![](/assets/images/1.png)

The rule of 30 stand for:
- A package shouldn’t contain more than 30 classes
- A class should contain an average of less than 30 methods
- A class should contain an average of less than 30 variables/properties/events
- Methods should not have more than an average of 30 code lines (not counting line spaces and comments)

**Note:**
Theses metrics are based on several theoretical research, knowing that the best maximum limit for a method or function is the number of lines that can fit on one screen, or that code with shorter routines has fewer defects overall, which matches with most people’s intuition and experience.
