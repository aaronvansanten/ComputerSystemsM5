# Module introduction
_2022-09-05_

## Coordinators:
- Erik Tews
	- Teaches operating systems
	- Bit of project
- Ana(-lucia) Varbanescu
	- New at UT

## Module 5 to-do list:
- Understand the basics of computer systems in terms of architecture, organization, and operation.
- Learn about legal aspects relevant to the IT domain.
- Learn the basics of discrete mathematics.
- Cooperate to design and realize a computer system integrating the topics from the first 8 weeks.
- Improve your academic skills, system development skills, concurrency and security skills.

## The topics
- Computer architecture and organizaiton
	- principles and practice of how computers are build and function (jointly with EE)
- Operating systems
	- principles and practice of how an operating systems works at the interface between HW and SW (using Linux)
- Project
	- Put everything together and build something cool with the RPi
- IT & Law
	- basics of juridical aspects and laws relevant to IT specialists, _some more_
- _some stuff about discrete mathematics that went too fast_

## Grading
- grades per study unit
- >5.5 per topic
- For OS: grade = 4/7 * course + 3/7 * project (but both must be >5.5)
- Stats from last year:
	- 60-70% passed each study unit
	- Average grade between 6 and 7
		- Lower for CAO, higher for projects
	- 10's have been given (in every topic)
	- Resit percentages were similar (also to 60-70% passing)

## Feedback from last year
_only writing down the interesting things_
- Too little free time
- Not clear how CAO fits in the picture
- Most challenging module so far
- Mixed comments on:
	- Topics integration
	- Video materials
	- Communication (Zulip vs. the world)
	- Project management
	- Organizational issues

## On-campus or online?
- Each course has chosen their own mix of activities
- Be ready for:
	- _A whole lot of things, I'm not going to list them all_

## Covid
- Officialy Covid is still a thing so keep that in mind, check the UTs current policy
- Keep in mind there are students who prefer to stay on the safe side or are from different cultures in this regard

## Communication
- _standard stuff_
- Using a Zulip server for (almost) the entire module instead of Discord _(Job approves, it's open source and has a Linux app)_
	- Will be an introduction in the OS course to Zulip
- Lectures are streamed with Zoom
- Recordings are not available by default (each teacher can choose for themselves)
- Pre-recorded lecture on YouTube, Vimeo, and Canvas

## Study advisors session
- _Boring, only for TCS students_
- _But they said something about being able to start your master in 2nd quartile, need to figure that out_

## Discrete mathematics
- How to formalize mathematics and communicate math
	- It's not deep math, more formalization
- _Teacher is quite new at the UT, hasn't done this module yet_
- 3 EC - 84 hours
- 13 lectures
- Book: "Discrete and Combinatorial Mathematics and its Applications" (Ralph P. Grimaldi; 5th edition, ISBN: 9781292022796)
- Prepare content of lectures by watching recordings and reading the book
- Content is reinforced in interactive lectures and tutorials
- Interactive lectures:
	- First summary and stimulating questions from the teachers
	- Then room foor questions from students _(he really emphasises that we should ask questions)_
	- Tutorials: work on exercises before and during -> TAs available during tutorials
- Course is extension of "introduction to mathematics"
- Exercises in column "Revision intro to math" are for repetition and not necessary
	- All other exercises not finished in the tutorials are homework

### Content:
- Part 1:
	- Chapter 2: logic
	- Chapter 3: Sets
- Part 2:
	- Chapter 4: mathematical induction
	- Chapter 5: relations and functions
	- Chapter 7: relations (continued)
Where chapter 2, 3 and 4 are extensions from the previous math course

### Exams:
- 2 partials of 1 hour right after each other
- 1combined resit
- no electronic devices allowed
- formula-sheets handed out at exam

### What you actually learn:
- Precise and formal communication
- Tools to work with formalized arguments _(insert weird equation which is really simple, we're gonna need to figure out how to type this in obsidian)_
- Solving problems and communicating the solution
- Given the high number of students, he will ignore most requests by email -> ask questions in interactive lectures and tutorials

Links to recorded lectures can be found in timetable **on canvas**.

## Operating Systems Introduction
Erik Tews
- Online lectures
	- Zoom/Twitch/YouTube
	- _Other places for recorded stuff, blah blah blah_

### Zulip introduction
- Kinda like discord, but also has topics (like stackoverflow/online forums)
- Threads/topics within streams
- Thereby you can filter per topic
- Latex and syntax highlighting (same way as in obsidian)
- Quote other people and reference other topics and threads
- No voice channels

### RPi introduction
_nothing new, we get a kit_
**What we need to provide ourselves:**
- way to connect Pi to laptop using ethernet
- SD card reader
- Empty USB thumb drive
- If you like: get an additional SD card for the project

### What is Linux
- Operating System
	- Some people say it's just a kernel, part of an OS
- Not suitable for very low end devices (Arduino) and certification mandatory systems

### General course structure
- Low level code for RPi
- Operating System
- Security in Linux

### Grading
- Each category about 25 assignments
- Solve as many assignments as you can and submit answer in writing
- For each of the categories you are invited for an interview with a TA wo goes through your answer
- For each accepted answer you get a point (min. 10 points per interview, and manage to implement an exploit (for security))
- Minimum 55 points to pass the course
- There is resit exam for those who fail in the interviews

- For Labs: you can be there whenever you want, for how long you want, but don't expect that all problems will be solved if you come in the last ten minutes

### What we should do
- Learn about C
	- Linux kernel in C
	- standard libraries and utitlities are often in C as well
	- Very powerful and common language
	- Not particularly safe and modern language
	- Not necessary to be a skilled developer, but do know the basics
	- For new project a better choice might be Rust (and C++)
- If you like:
	- Assembly for ARM64, C++, Rust
- Get the Pi early and to the introduction early in case something is wrong
- Install Raspbian 64 bit
