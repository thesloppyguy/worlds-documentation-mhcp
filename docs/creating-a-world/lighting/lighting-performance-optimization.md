# Horizon Worlds Performance Optimization: Accurate Guide

**Optimize your Horizon Worlds for smooth performance across VR, mobile, and web platforms.** This guide covers real performance monitoring tools, optimization techniques, and troubleshooting based on actual Horizon Worlds capabilities.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Understanding of Horizon Worlds building fundamentals and basic optimization concepts.

**Estimated Time to Complete**
1 hour for core optimization techniques, 30 minutes for mobile-specific optimizations

## Table of Contents

1. [Performance Monitoring with Real-time Metrics](#performance-monitoring-with-real-time-metrics)
2. [Memory Management and Limits](#memory-management-and-limits)
3. [Cross-Platform Optimization](#cross-platform-optimization)
4. [Mobile and Web Optimization](#mobile-and-web-optimization)
5. [Performance Troubleshooting](#performance-troubleshooting)
6. [Best Practices](#best-practices)

## Performance Monitoring with Real-time Metrics

Horizon Worlds provides a built-in Real-time Metrics panel to monitor your world's performance.

### **Accessing Real-time Metrics:**

**In VR:**

1. Enable **Utilities** menu in Settings
2. Open wrist menu and select **Real-time Metrics**

**On Web:**

- Press **P** key to toggle metrics panel

**Not Available:**

- Desktop editor (use VR or web for testing)
- Mobile (testing must be done in VR or web)

### **Key Performance Metrics:**

| Metric         | Target            | Description                              |
| -------------- | ----------------- | ---------------------------------------- |
| **FPS**        | 72 (VR), 60 (Web) | Frames per second - most critical metric |
| **CPU**        | <13.8ms (VR)      | CPU processing time per frame            |
| **GPU**        | <13.8ms (VR)      | GPU rendering time per frame             |
| **Memory**     | <6.25 GB          | Total memory usage (hard limit)          |
| **Draw Calls** | Minimize          | Number of render batches                 |
| **Vertices**   | Monitor           | Total vertices rendered per frame        |
| **Physics**    | Monitor           | Physics simulation time                  |
| **Scripting**  | Monitor           | TypeScript execution time                |

### **Application Space Warp (ASW):**

- **ASW Value 0**: Off (normal performance targets)
- **ASW Value 1**: On (doubles frame budget, relaxes targets)
- Automatically enabled when **Frame Budget Boost** is on
- Helps maintain smooth experience when performance drops

### **Phase Sync Time:**

- **High value**: World performing well (generating frames faster than needed)
- **0ms**: World struggling to maintain target FPS
- Acts as performance buffer

## Memory Management and Limits

**Critical**: Horizon Worlds enforces a **6.25 GB memory limit** per world (introduced July 2024).

### **Memory Limit Enforcement:**

- Worlds exceeding 6.25 GB **cannot be published**
- Cannot add new objects/assets when limit is reached
- Existing worlds must be optimized to stay under limit
- System may crash worlds that exceed memory limits

### **Memory Optimization Strategies:**

**Reduce Asset Size:**

- Use compressed textures appropriate for target resolution
- Optimize 3D models (reduce polygon count)
- Remove unused materials and textures
- Use texture atlasing to combine materials

**Efficient Asset Usage:**

- Reuse materials across multiple objects
- Share geometries when possible
- Remove duplicate assets
- Use appropriate LOD (Level of Detail) models

**Monitor Memory Usage:**

- Use Real-time Metrics panel to track memory
- Set memory target alerts (e.g., 5.5 GB warning)
- Test memory usage throughout development
- Profile memory usage during peak activity

## Cross-Platform Optimization

Optimize for VR, mobile, and web platforms simultaneously for maximum reach.

### **Platform-Specific Targets:**

| Platform       | FPS Target | Primary Considerations                    |
| -------------- | ---------- | ----------------------------------------- |
| **VR (Quest)** | 72 FPS     | High performance, comfort, 6 GB RAM       |
| **Web**        | 60 FPS     | Varies by browser and hardware            |
| **Mobile**     | 30+ FPS    | Limited processing power, touch interface |

### **Cross-Platform Design Principles:**

**Performance Scaling:**

- Design for lowest common denominator (mobile)
- Use Environment Gizmo presets that work across platforms
- Minimize dynamic lights (20 maximum limit affects all platforms)
- Keep draw calls low through material batching

**Interface Considerations:**

- Text must be legible on mobile screens
- Touch-friendly interaction areas
- Screen-based UI for important information
- VFX and audio cues for non-text communication

**Discovery Benefits:**

- Mobile-optimized worlds get **priority in discovery**
- Cross-platform worlds reach wider audiences
- Mobile/web players can play with VR players

## Mobile and Web Optimization

Mobile and web platforms require specific optimization strategies.

### **Mobile-Specific Optimizations:**

**Visual Quality Adjustments:**

- Use simpler Environment Gizmo presets
- Reduce fog density (lower values like 0.02-0.05)
- Minimize particle effects
- Use fewer dynamic lights
- Optimize texture sizes for mobile screens

**Interface Design:**

- Large, touch-friendly interaction areas
- Clear visual feedback for touch interactions
- Screen-based UI for critical information
- Avoid relying solely on spatial audio cues

**Performance Considerations:**

- Target 30+ FPS minimum on mobile
- Monitor memory usage more closely
- Test on actual mobile devices
- Consider slower mobile processors

### **Web Optimization:**

**Browser Compatibility:**

- **Supported**: Chrome, Safari, Edge
- **Not Supported**: Firefox
- Test across supported browsers
- Account for varying hardware capabilities

**Performance Targets:**

- 60 FPS target for web
- Variable performance based on user hardware
- Optimize for integrated graphics
- Consider network bandwidth for assets

### **Mobile Discovery Advantages:**

Mobile-optimized worlds receive:

- **Enhanced discovery placement**
- **"Jump back in" shelf** visibility
- **Direct world search** functionality
- **Cross-platform session continuity**

## Performance Troubleshooting

Common performance issues and their solutions.

### **Low FPS Diagnosis:**

**Step 1: Check Real-time Metrics**

- Open metrics panel (Utilities menu in VR, P key on web)
- Identify which metric is exceeding targets

**Step 2: Common Issues and Solutions**

| Issue           | Likely Cause               | Solution                                    |
| --------------- | -------------------------- | ------------------------------------------- |
| High CPU time   | Too many scripts/physics   | Optimize TypeScript, reduce physics objects |
| High GPU time   | Complex geometry/materials | Reduce polygons, optimize textures          |
| Low Phase Sync  | Multiple bottlenecks       | Address CPU and GPU issues first            |
| High Memory     | Too many/large assets      | Remove unused assets, compress textures     |
| High Draw Calls | Too many materials         | Batch materials, use texture atlasing       |
| High Vertices   | Complex geometry           | Use LOD models, reduce polygon count        |

### **Memory Issues:**

**Symptoms:**

- Cannot add new objects
- Cannot publish world
- World crashes during play

**Solutions:**

- Delete unused assets
- Compress existing textures
- Reduce polygon count on models
- Remove duplicate materials
- Use invisible glow objects instead of additional dynamic lights

### **Cross-Platform Issues:**

**Mobile Performance Problems:**

- Reduce environmental complexity
- Use simpler Environment Gizmo presets
- Minimize dynamic effects
- Test on actual mobile devices

**Web Browser Issues:**

- Check browser compatibility (Chrome, Safari, Edge only)
- Optimize for integrated graphics
- Reduce asset sizes for faster loading

## Best Practices

### **Performance Development Workflow:**

1. **Monitor Throughout Development**

   - Check Real-time Metrics regularly
   - Set memory alerts at 5.5 GB
   - Test on target platforms frequently

2. **Optimize Early and Often**

   - Profile performance during development
   - Address issues as they arise
   - Don't wait until final testing

3. **Test Across Platforms**
   - VR testing for primary experience
   - Web testing for browser compatibility
   - Mobile testing (actual devices when possible)

### **Asset Management:**

**Do's ✅**

- Compress textures appropriately
- Reuse materials and geometries
- Use texture atlasing for small textures
- Remove unused assets regularly
- Monitor memory usage constantly

**Don'ts ❌**

- Don't exceed 6.25 GB memory limit
- Don't use excessive dynamic lights (20 max)
- Don't ignore mobile performance
- Don't rely on fictional performance APIs
- Don't over-optimize at expense of core experience

### **Platform Optimization Checklist:**

**Before Publishing:**

- [ ] FPS targets met: 72 (VR), 60 (Web), 30+ (Mobile)
- [ ] Memory usage under 6.25 GB limit
- [ ] Real-time Metrics show healthy performance
- [ ] Cross-platform testing completed
- [ ] Mobile text legibility confirmed
- [ ] Touch interactions work properly
- [ ] Works in supported browsers (Chrome, Safari, Edge)

### **Common Mistakes to Avoid:**

1. **Trying to create custom performance monitoring** - Use built-in Real-time Metrics panel
2. **Ignoring memory limits** - 6.25 GB is a hard limit, plan accordingly
3. **Not testing on mobile** - Mobile optimization requires actual device testing
4. **Using fictional APIs** - No WorldSettings class or custom monitoring systems exist
5. **Optimizing too late** - Monitor performance throughout development

### **Discovery and Reach:**

**Mobile-First Benefits:**

- Priority placement in discovery surfaces
- Access to "Jump back in" shelf
- Direct world search functionality
- Broader audience reach
- Cross-platform play sessions

**Optimization ROI:**

- Mobile optimization = better discovery
- Better discovery = more players
- More players = better community feedback
- Cross-platform compatibility = maximum reach

## Advanced Optimization Techniques

### **Memory Optimization:**

**Asset Streaming:**

- Load assets as needed (if using custom scripts)
- Remove objects from scenes when not needed
- Use object pooling for repeated elements

**Texture Optimization:**

- Use appropriate texture sizes for target screens
- Compress textures without visible quality loss
- Use texture atlasing for UI elements
- Remove unused texture channels

### **Performance Profiling:**

**Systematic Approach:**

1. Establish baseline metrics in empty world
2. Add content incrementally
3. Monitor impact of each addition
4. Address issues before adding more content
5. Test final optimization on all platforms

**Bottleneck Identification:**

- CPU bound: Optimize scripts, reduce physics
- GPU bound: Reduce geometry complexity, optimize materials
- Memory bound: Remove assets, compress textures
- Mixed: Address highest impact issues first

## Next Steps

After implementing these optimizations:

1. **Regular Monitoring**: Check performance metrics weekly
2. **Community Testing**: Get feedback from players on different platforms
3. **Iterative Improvement**: Continuously optimize based on real-world usage
4. **Platform Updates**: Stay informed about Horizon Worlds platform changes
5. **Best Practice Sharing**: Share successful optimization strategies with community

---

**Ready to Optimize Your World?**

Focus on using the actual tools available in Horizon Worlds rather than fictional APIs. The Real-time Metrics panel provides all the data you need to create high-performing, cross-platform experiences that reach the widest possible audience.

**Remember**: The 6.25 GB memory limit and 20 dynamic light limit are hard constraints. Plan your world design around these limitations from the beginning rather than trying to optimize later.

---

_This guide reflects the actual capabilities and limitations of Meta Horizon Worlds as of 2025. For the latest updates and features, visit the [official Horizon Worlds documentation](https://developers.meta.com/horizon-worlds/)._
