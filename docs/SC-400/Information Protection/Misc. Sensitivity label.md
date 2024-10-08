
## Legacy and non-standard formats:

When applying Microsoft Purview Sensitivity Labels to older binary file formats (`.doc`, `.xls`, `.ppt`), there are specific limitations compared to modern Office formats (`.docx`, `.xlsx`, `.pptx`). Here are the exact limitations:

### 1. **No Persistent Labeling in Binary Formats**
   - In older binary formats (`.doc`, `.xls`, `.ppt`), the sensitivity label is not embedded directly into the file as metadata. This means:
     - The label is not persistent if the file is moved or copied.
     - If a labeled binary file is saved in one of the modern formats, the label needs to be applied again.

### 2. **Limited Encryption and Protection**
   - The full encryption and protection features of sensitivity labels are not available for binary formats. This includes:
     - Automatic encryption and access controls (e.g., setting permissions based on the label) may not be applied as effectively.
     - Some advanced rights management options, like expiration settings, are not supported in binary formats.

### 3. **Content Markings (Headers, Footers, Watermarks)**
   - The application of content markings like headers, footers, and watermarks is either limited or not supported in the older binary formats.
   - Modern formats allow for better handling of visual markings (e.g., "Confidential" in the header), but these may not appear correctly in older file types.

### 4. **Limited Content Scanning and Auto-Labeling**
   - Auto-labeling based on content scanning (where labels are applied automatically based on keywords or patterns) is not supported for binary formats.
   - These features rely on the ability to inspect the document's structure, which is much more efficient in modern XML-based formats.

### 5. **No Unified Label Viewer Experience**
   - Users opening older binary files may not see the same user interface experience for sensitivity labels in their Office applications as they would with modern formats. This might include:
     - Lack of visual indicators (like labels or banners) within the document.
     - Less integration with modern compliance reporting tools.

### 6. **Performance Issues**
   - Applying sensitivity labels or enforcing compliance policies may be slower or less reliable with older binary formats compared to modern file types.

### 7. **No Support for Co-authoring**
   - Co-authoring (multiple people editing a document simultaneously) is only supported in modern Office file formats. Older binary formats do not support this feature, which also affects the ability to maintain sensitivity labels during collaboration.

### Recommendation:
While Microsoft Purview Sensitivity Labels can be applied to legacy binary formats, the experience and security features are significantly reduced. To fully leverage all capabilities of Microsoft Purview (e.g., persistent labeling, full encryption, auto-classification, co-authoring), it's recommended to convert binary formats to the modern equivalents (`.docx`, `.xlsx`, `.pptx`).