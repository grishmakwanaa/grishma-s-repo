# ColorSync AI - Next.js Application

A beautiful, production-ready Next.js application for AI-powered color palette selection for outfits and makeup.

## Features

- 🎨 Modern, elegant design with smooth animations
- 📱 Fully responsive (mobile, tablet, desktop)
- ⚡ Built with Next.js 14 App Router
- 🎯 TypeScript for type safety
- 💅 CSS Modules for scoped styling
- 🖼️ Image upload and preview
- 🤖 Ready for ML model integration
- ✨ Professional UI/UX with Google Fonts

## Tech Stack

- **Framework:** Next.js 14 (App Router)
- **Language:** TypeScript
- **Styling:** CSS Modules
- **Fonts:** Google Fonts (Cormorant Garamond, DM Sans)

## Project Structure

```
colorsync-nextjs/
├── app/
│   ├── api/
│   │   └── analyze/
│   │       └── route.ts          # API endpoint for ML model
│   ├── layout.tsx                # Root layout with fonts
│   └── page.tsx                  # Main page
├── components/
│   ├── Navigation.tsx            # Navigation bar
│   ├── Navigation.module.css
│   ├── Hero.tsx                  # Hero section
│   ├── Hero.module.css
│   ├── Features.tsx              # Features showcase
│   ├── Features.module.css
│   ├── UploadSection.tsx         # Photo upload interface
│   ├── UploadSection.module.css
│   ├── ResultsSection.tsx        # Color results display
│   ├── ResultsSection.module.css
│   ├── Footer.tsx                # Footer
│   └── Footer.module.css
├── styles/
│   └── globals.css               # Global styles
├── public/                       # Static assets
├── package.json
├── tsconfig.json
└── next.config.js

```

## Getting Started

### Prerequisites

- Node.js 18+ installed
- npm or yarn package manager

### Installation

1. **Install dependencies:**

```bash
npm install
# or
yarn install
```

2. **Run the development server:**

```bash
npm run dev
# or
yarn dev
```

3. **Open your browser:**

Navigate to [http://localhost:3000](http://localhost:3000)

## Integrating Your ML Model

The application is designed to easily integrate with your machine learning model. Here's how:

### Option 1: API Endpoint Integration

Update the `handleFileUpload` function in `/components/UploadSection.tsx`:

```typescript
const handleFileUpload = async (event: ChangeEvent<HTMLInputElement>) => {
  const file = event.target.files?.[0]
  if (!file) return

  setLoading(true)

  // Read and display the image
  const reader = new FileReader()
  reader.onload = (e) => {
    setUploadedImage(e.target?.result as string)
  }
  reader.readAsDataURL(file)

  // Call your ML API
  const formData = new FormData()
  formData.append('image', file)
  
  try {
    const response = await fetch('/api/analyze', {
      method: 'POST',
      body: formData
    })
    
    const data = await response.json()
    setResults(data.colors)
    setLoading(false)
    
    // Scroll to results
    setTimeout(() => {
      document.getElementById('results')?.scrollIntoView({ behavior: 'smooth' })
    }, 100)
  } catch (error) {
    console.error('Error:', error)
    setLoading(false)
  }
}
```

### Option 2: Update the API Route

Modify `/app/api/analyze/route.ts` to connect to your ML model:

```typescript
export async function POST(request: NextRequest) {
  try {
    const formData = await request.formData()
    const image = formData.get('image') as File
    
    // Convert to base64 or send directly to your ML model
    const buffer = await image.arrayBuffer()
    const base64Image = Buffer.from(buffer).toString('base64')
    
    // Call your ML model API
    const mlResponse = await fetch('YOUR_ML_MODEL_ENDPOINT', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${process.env.ML_API_KEY}`
      },
      body: JSON.stringify({
        image: base64Image,
        // Add other parameters as needed
      })
    })
    
    const mlData = await mlResponse.json()
    
    return NextResponse.json({
      colors: mlData.colors,
      skinTone: mlData.skinTone,
      season: mlData.season
    })
    
  } catch (error) {
    console.error('Error processing image:', error)
    return NextResponse.json(
      { error: 'Failed to process image' },
      { status: 500 }
    )
  }
}
```

### Expected ML Model Response Format

Your ML model should return data in this format:

```json
{
  "colors": [
    {
      "name": "Warm Terracotta",
      "hex": "#c77743",
      "usage": "Perfect for autumn outfits and evening wear",
      "confidence": 0.95
    }
  ],
  "skinTone": "warm",
  "season": "autumn"
}
```

## Environment Variables

Create a `.env.local` file in the root directory:

```env
# ML Model API Configuration
ML_API_KEY=your_api_key_here
ML_API_ENDPOINT=https://your-ml-api-endpoint.com
```

## Building for Production

```bash
npm run build
npm start
```

## Deployment

### Deploy to Vercel (Recommended)

1. Push your code to GitHub
2. Import your repository on [Vercel](https://vercel.com)
3. Add your environment variables
4. Deploy!

### Deploy to Other Platforms

The application can be deployed to any platform that supports Next.js:

- **Netlify:** Use the Next.js plugin
- **AWS Amplify:** Supports Next.js SSR
- **Railway:** One-click Next.js deployment
- **Docker:** Create a Docker container

## Customization

### Colors

Update the CSS variables in `/styles/globals.css`:

```css
:root {
  --primary-dark: #1a1a1a;
  --primary-light: #f8f5f0;
  --accent-rust: #c77743;
  --accent-sage: #8b9d83;
  --accent-cream: #e8dcc8;
  --accent-plum: #6b4e71;
}
```

### Fonts

Change fonts in `/app/layout.tsx` by importing different Google Fonts.

### Content

Update text content directly in the component files:
- Hero text: `/components/Hero.tsx`
- Features: `/components/Features.tsx`
- Footer: `/components/Footer.tsx`

## Performance Optimization

- ✅ Google Fonts optimized with `next/font`
- ✅ CSS Modules for code splitting
- ✅ Image optimization ready (use `next/image` when needed)
- ✅ Server-side rendering enabled
- ✅ Lazy loading for results section

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers

## Contributing

Feel free to fork this project and customize it for your needs!

## License

MIT License - feel free to use this for your project.

## Support

For issues or questions, please open an issue on GitHub or contact the development team.

---

Built with ❤️ using Next.js and TypeScript
