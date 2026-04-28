Performance testing screenshots

1. test_plan_1.jmx
![test_plan_1](images/test_plan_1.jmx.png)
![test_results_1](images/testresults1.jtl.png)

2. test_plan_2.jmx
![test_plan_2](images/test_plan_2.jmx.png)
![test_results_2](images/testresults2.jtl.png)

3. test_plan_3.jmx
![test_plan_3](images/test_plan_3.jmx.png)
![test_results_3](images/testresults3.jtl.png)

## Profiling and Performance Optimization

### 1. `getAllStudentsWithCourses` Endpoint
- **Before Optimization:** 4,179 ms
- **After Optimization:** 1,758 ms
- **Improvement:** 2,421 ms (Faster by **`~58%`**)

![Before Optimization](images/all_student_with_course.png)
![After Optimization](images/all_student_with_course_optimized.png)


### 2. `joinStudentNames` Endpoint
- **Before Optimization:** 710 ms
- **After Optimization:** 304 ms
- **Improvement:** 406 ms (Faster by **`~57%`**)

![Before Optimization](images/student_name.png)
![After Optimization](images/student_name_optimized.png)


### 3. `findStudentWithHighestGpa` Endpoint
- **Before Optimization:** 234 ms
- **After Optimization:** 152 ms
- **Improvement:** 82 ms (Faster by **`~35%`**)

![Before Optimization](images/highest_gpa.png)
![After Optimization](images/highest_gpa_optimized.png)


### Conclusion
As observed from the profiling results above, the initial implementations suffered from heavy memory usage and excessive database hits (such as the N+1 problem and creating a large number of internal loops natively in Java). 

The refactoring process pushed the heavy lifting down to the database level (employing built-in JPA repository queries) and leveraged smarter native data streams (like Java 8 `Collectors.joining`). This significantly reduced execution times across the board—producing performance boosts between **35% and 58%**, which easily exceeds the >20% requirement target. Consequently, the application operates faster, uses computational resources more efficiently, and provides a much more responsive user experience overall.

---

## Reflection

**1. What is the difference between the approach of performance testing with JMeter and profiling with IntelliJ Profiler in the context of optimizing application performance?**  
JMeter tests the application from the *outside* by simulating many users sending requests (load testing) to see how the server responds under pressure. Meanwhile, IntelliJ Profiler looks from the *inside* to see exactly which lines of code or methods take up the most time and memory during execution.

**2. How does the profiling process help you in identifying and understanding the weak points in your application?**  
It eliminates guessing. By looking at the profiler's tools (like Flame Graphs and Call Trees), I can visually see what methods are taking too long to process, for example, spotting an internal loop that is making thousands of unneeded database calls.

**3. Do you think IntelliJ Profiler is effective in assisting you to analyze and identify bottlenecks in your application code?**  
Yes, it's very effective. The visual representation makes it extremely easy for a beginner like me to spot the heaviest parts of the code quickly, without having to manually read and guess which part is dragging the performance down.

**4. What are the main challenges you face when conducting performance testing and profiling, and how do you overcome these challenges?**  
The main challenge is getting overwhelmed by the sheer amount of data and built-in Java/Spring library methods shown in the profiler. I overcome this by filtering the results or focusing only on my application's package name (`com.advpro...`) and looking at the widest bars in the flame graph.

**5. What are the main benefits you gain from using IntelliJ Profiler for profiling your application code?**  
The main benefit is gaining deep visibility into how my code actually runs. It helps me pinpoint exact inefficiencies (like the N+1 problem) and provides pure data (ms) as proof to verify that my optimizations genuinely worked.

**6. How do you handle situations where the results from profiling with IntelliJ Profiler are not entirely consistent with findings from performance testing using JMeter?**  
I remember that JMeter includes network connection and server latency, while the Profiler only measures internal JVM code execution. If they don't match, I assume the bottleneck might not be in the code itself, but rather in the network, OS limitations, or database connection pool settings.

**7. What strategies do you implement in optimizing application code after analyzing results from performance testing and profiling? How do you ensure the changes you make do not affect the application's functionality?**  
My main strategies are letting the database do the heavy lifting (using proper JPA queries instead of Java `for` loops) and using efficient tools (like Java Streams/StringBuilder instead of simple String `+` operations). To ensure I don't break anything, I rely on automated test cases (Unit/Integration Tests) and compare the endpoint's JSON response formats before and after making code changes.

