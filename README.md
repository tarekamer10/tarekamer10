<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Face Analysis Service</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Home Page -->
    <section id="home">
        <header>
            <h1>You’re Not Ugly—You Just Haven’t Unlocked Your True Features!</h1>
            <p>Discover how your unique facial features align with the golden dimensions of beauty and unlock your true potential.</p>
            <button onclick="location.href='#upload'">Upload Your Photo Now</button>
            <p>Get a personalized report with actionable insights instantly.</p>
        </header>
        <div class="features">
            <h2>Key Features</h2>
            <ul>
                <li>Face Rating System: Compare your features to scientifically defined "perfect dimensions."</li>
                <li>Improvement Insights: Learn your potential score and how to achieve it.</li>
                <li>Non-Surgical Enhancement Tips: Get detailed advice on fillers, skincare, and natural beauty solutions.</li>
                <li>Lifestyle Recommendations: Tailored tips on diet, vitamins, and daily habits to enhance your look and feel.</li>
            </ul>
        </div>
    </section>

    <!-- How It Works Page -->
    <section id="how-it-works">
        <h2>How It Works</h2>
        <ol>
            <li>
                <h3>Upload Your Photo</h3>
                <p>Upload a clear picture of your face. Don’t worry—your privacy is our priority, and your data is fully secure.</p>
            </li>
            <li>
                <h3>Advanced AI Analysis</h3>
                <p>Our AI technology analyzes your unique features against ideal proportions and symmetry metrics.</p>
            </li>
            <li>
                <h3>Personalized Report</h3>
                <p>Receive an in-depth report detailing your current rating, potential improvements, and tailored recommendations for achieving your best look.</p>
            </li>
        </ol>
        <button onclick="location.href='#upload'">Start Your Analysis Today!</button>
    </section>

    <!-- Pricing Page -->
    <section id="pricing">
        <h2>Affordable Solutions for Unlocking Your Best Look</h2>
        <div class="pricing-options">
            <div class="plan">
                <h3>Basic Analysis</h3>
                <p>Current facial rating.</p>
                <p>Overview of golden proportions.</p>
                <p><strong>$19.99</strong></p>
                <button>Choose Plan</button>
            </div>
            <div class="plan">
                <h3>Advanced Analysis</h3>
                <p>Current and potential ratings.</p>
                <p>Non-surgical improvement tips.</p>
                <p>Lifestyle and vitamin recommendations.</p>
                <p><strong>$49.99</strong></p>
                <button>Choose Plan</button>
            </div>
            <div class="plan">
                <h3>Premium Consultation</h3>
                <p>Everything in Advanced.</p>
                <p>1-on-1 virtual consultation with an expert.</p>
                <p>Customized improvement plan.</p>
                <p><strong>$149.99</strong></p>
                <button>Choose Plan</button>
            </div>
        </div>
    </section>

    <!-- Contact Us Page -->
    <section id="contact">
        <h2>We’re Here to Help</h2>
        <form action="#" method="post">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name" required>

            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>

            <label for="message">Message:</label>
            <textarea id="message" name="message"></textarea>

            <label for="photo">Upload Photo (Optional):</label>
            <input type="file" id="photo" name="photo">

            <button type="submit">Submit Your Query</button>
        </form>
        <p>Contact us at support@[yourdomain].com or call 1-800-XXX-XXXX.</p>
    </section>

</body>
</html>
