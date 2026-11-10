If you can't understand and explain the code, don't ship it
1. **Verify every line:** If you cannot explain the code line-by-line, it should not be merged. Understand your "verification debt" 
2. **Search before you write:** Check the codebase for existing utilities to avoid unnecessary code duplication
3. **Don't mask errors:** Do not swallow exceptions in `try-catch` blocks; handle them meaningfully or let them fail loudly
4. **Treat input as hostile:** Assume all user input, data, and parameters are potential attacks and validate at the boundary
5. **Reproduce before fixing:** Always create a failing test case to confirm a bug before attempting a fix 
6. **Shrink the blast radius:** Use feature flags and gradual rollouts to reduce the impact of potential mistakes 
7. **Document the 'why':** Explain the reasoning behind code decisions in comments, commit messages, or decision records
8. **Vet dependencies:** Never install a package you haven't checked yourself to prevent "slop-squatting" and security risks