```index.html
            const output = document.querySelectorAll("pre")[0];
            const sen = document.getElementById("sen");

            function sfc32(a) {
                return function () {
                    a |= 0;
                    a = (a + 0x9e3779b9) | 0;
                    var t = a ^ (a >>> 16);
                    t = Math.imul(t, 0x21f0aaad);
                    t = t ^ (t >>> 15);
                    t = Math.imul(t, 0x735a2d97);
                    return ((t = t ^ (t >>> 15)) >>> 0) / 4294967296;
                };
            }

            const today = new Date();
            const seed =
                today.getFullYear() * 10000 +
                (today.getMonth() + 1) * 100 +
                today.getDate();
            const rand = sfc32(seed);

            function generateDigits(count) {
                let digits = "";
                for (let i = 0; i < count; i++) {
                    // Generate a random integer 0-9
                    digits += Math.floor(rand() * 10);
                }
                output.textContent += digits;
            }

            // Initial 1000 digits
            generateDigits(100000);

            const obs = new IntersectionObserver(
                (entries) => {
                    if (entries[0].isIntersecting) {
                        generateDigits(20000);
                    }
                },
                {
                    rootMargin: "400px",
                },
            );

            obs.observe(sen);
```

```archives.html
        function mulberry32(seed) {
            return function () {
                let t = (seed += 0x6d2b79f5);
                t = Math.imul(t ^ (t >>> 15), t | 1);
                t ^= t + Math.imul(t ^ (t >>> 7), t | 61);
                return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
            };
        }

        document.getElementById("go").addEventListener("click", function () {
            const dateInput = document.getElementById("to").value;
            if (!dateInput) {
                alert("Enter a valid date.\n\nWasted computation is not acceptable.");
                return;
            }

            const seed = parseInt(dateInput.replace(/-/g, ""), 10);
            const random = mulberry32(seed);
            const result = Math.floor(random() * 10000000000000000);
            const paddedResult = result.toString().padStart(16, "0");
            window.location.href = "./archives/mnemosyne/" + paddedResult;
        });
```
