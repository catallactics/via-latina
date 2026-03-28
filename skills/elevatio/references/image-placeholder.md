# Image Placeholder Handling

## Purpose

Find and handle `[!image-needed]` placeholders during project creation/promotion.

## Placeholder Format

```markdown
> [!image-needed] {description}
> - Keywords: {detected keywords}
> - ASCII: {yes/no}
> - Recommended: {photo type}
```

## Search Pattern

```bash
grep -r "\[!image-needed\]" {project-folder}/
```

## Workflow

1. **After project creation**: Scan all `.md` files in project folder
2. **If found**: Display summary to user
   ```
   3 images needed in this project:
   - notes/n05. Breadboard Guide.md: LED connection photo
   - refined/r01. Arduino Log.md: circuit diagram
   - refined/r01. Arduino Log.md: sensor wiring photo
   ```
3. **User decides**:
   - Add images now → remove placeholder after adding
   - Defer → placeholders remain searchable

## Placeholder Removal

After image added, replace placeholder with actual image:

**Before:**
```markdown
> [!image-needed] Breadboard LED connection photo
> - Keywords: breadboard, LED
> - ASCII: yes
> - Recommended: actual photo
```

**After:**
```markdown
![[breadboard-led-connection.jpg]]
```

## Notes

- Placeholders created by purgatio during note refinement
- elevatio only searches and notifies, doesn't create placeholders
- User can search vault-wide: `grep -r "\[!image-needed\]" vault/`
