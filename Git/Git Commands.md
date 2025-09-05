# حذف فایل/پوشه از ریموت (اما نگه داشتن در لوکال)
\> git rm -r --cached path/to/folder-or-file

git filter-repo --path Asset/ --invert-paths --force


برای حذف remote موجود (مثلاً `origin`) و اضافه کردن ریموت جدید از دستور زیر استفاده کنید:
git remote -v   // مشاهده ریموتهای موجود
git remote remove origin
git remote add origin https://github.com/sadeghi313b/coding-tutorials.git