# Image Cropper Module

This module provides a reusable image upload interface with built-in cropping functionality, optimized for jQuery + Bootstrap environments.

## 📁 File Structure

```
/common/image_cropper/
  ├── input.php         # Image input field with preview and crop button
  ├── modal.php         # Shared modal component for cropping
  └── readme.md         # This documentation
```

## 🧩 Usage Instructions

### 1. Include the modal once per page (outside any form)

```php
<?php $this->load->view('common/image_cropper/modal', [
	'title' => '商品画像アップロード'
]); ?>
```

### 2. Include the image input component for each field

```php
<?php $this->load->view('common/image_cropper/input', [
  'field' => 'admin_image_url',
  'label' => '管理者の画像',
  'default_image_url' => $admin_detail['admin_image_url'],
]); ?>
```

## 🔧 Available Parameters for `common/image_cropper/input`

| Variable            | Type    | Required | Description                                    |
| ------------------- | ------- | -------- | ---------------------------------------------- |
| `field`             | string  | ✅ Yes   | Input name                                     |
| `label`             | string  | ❌ No    | Input label                                    |
| `default_image_url` | string  | ❌ No    | Display preloaded image (if available)         |
| `disabled`          | boolean | ❌ No    | Disable image preview and upload functionality |

## 📝 Notes

- The modal should only be included once per page.
- All image input instances share the same modal, data is passed dynamically.

Feel free to extend the module with parameters like crop aspect ratio, max dimensions, etc.

---

For any further enhancements or issues, please contact the developer
