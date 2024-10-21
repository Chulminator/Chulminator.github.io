---
title: NeetCode - Course Schedule
author: chulmin
date: 2024-10-15 00:00:00 -0500
categories: [Daily Smashing Through Random Problems, Neetcode, Course Schedule]
tags: [coding test, c++, neetcode]
math: true
---

## Idea
### before solving the problem
I realized that the requirement to attend all classes can be represented as a graph, where each course is a node and the prerequisites are the edges connecting these nodes. This led me to think that a search algorithm would be an effective way to solve this problem.

### while solving the problem
During the implementation, I discovered that you can only take a class once you have completed all its prerequisites. This realization led me to adopt a vector called `numPrereq` to keep track of the number of prerequisites for each course.


## Code

``` c++
class Solution {
public:
    bool canFinish(int numCourses, std::vector<std::vector<int>>& prerequisites) {
        std::map<int, std::queue<int>> courses;
        std::vector<bool> visited(numCourses, false);
        std::queue<int> toVisit;
        std::vector<int> numPrereq(numCourses, 0);

        for (const auto& prereq : prerequisites) {
            courses[prereq[1]].push(prereq[0]);
            numPrereq[prereq[0]]++; 
        }

        for (int ii = 0; ii < numCourses; ++ii) {
            if (numPrereq[ii] == 0) {
                toVisit.push(ii);
            }
        }

        while (!toVisit.empty()) {
            int course = toVisit.front(); // you are taking this class
            toVisit.pop();
            if (visited[course]) {
                continue;
            }
            visited[course] = true;

            while (!courses[course].empty()) { // what othe classes can you take using the current course as a prerequisite
                int nextCourse = courses[course].front();
                courses[course].pop();
                numPrereq[nextCourse]--; 
                if (numPrereq[nextCourse] == 0) {
                    toVisit.push(nextCourse); 
                }
            }
        }

        for (int ii = 0; ii < numCourses; ++ii) {
            // std::cout << ii << ": " << visited[ii] << std::endl; 
            if (!visited[ii]) {
                return false;
            }
        }
        return true; 
    }
};
```


## Reference
- [Course schedule](https://neetcode.io/problems/course-schedule)