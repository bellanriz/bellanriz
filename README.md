import { useState, useEffect } from 'react';
import { motion } from 'motion/react';
import { Download } from 'lucide-react';
import { useTheme } from '../lib/ThemeContext';

const TITLES = [
  'Aspiring AI Engineer',
  'Fullstack AI Developer',
  'Software Engineer',
  'Machine Learning',
  'Computer Vision Engineer',
];

export function ProfileHeader() {
  const { theme } = useTheme();

  const [titleIndex, setTitleIndex] = useState(0);
  const [displayText, setDisplayText] = useState('');
  const [isDeleting, setIsDeleting] = useState(false);

  useEffect(() => {
    const currentTitle = TITLES[titleIndex];

    const typingSpeed = isDeleting ? 50 : 100;
    const pauseAfterTyping = 2000;

    const timeout = setTimeout(() => {
      // Finished typing
      if (!isDeleting && displayText === currentTitle) {
        setTimeout(() => {
          setIsDeleting(true);
        }, pauseAfterTyping);

        return;
      }

      // Finished deleting
      if (isDeleting && displayText === '') {
        setIsDeleting(false);
        setTitleIndex((prev) => (prev + 1) % TITLES.length);
        return;
      }

      // Typing
      if (!isDeleting) {
        setDisplayText(
          currentTitle.slice(0, displayText.length + 1)
        );
      }

      // Deleting
      else {
        setDisplayText(
          displayText.slice(0, -1)
        );
      }
    }, typingSpeed);

    return () => clearTimeout(timeout);
  }, [displayText, isDeleting, titleIndex]);

  return (
    <div className="p-10 border-b border-border bg-background transition-colors duration-300">

      {/* Profile */}
      <div className="flex items-center gap-6 mb-8">

        {/* Profile Image */}
        <div
          className={`
            w-24 h-24
            rounded-full
            overflow-hidden
            border-4
            shadow-xl
            relative
            group
            ${
              theme === 'dark'
                ? 'border-zinc-900 bg-zinc-900'
                : 'border-zinc-200 bg-zinc-100'
            }
          `}
        >
          <img
            src="/images/dclassic.jpg"
            alt="Nabilah Abas"
            className="w-full h-full object-cover"
          />

          {/* Hover Effect */}
          <div className="absolute inset-0 bg-black/40 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center">
            <div className="w-8 h-8 rounded-full bg-white/20 backdrop-blur-sm border border-white/30" />
          </div>
        </div>

        {/* Name */}
        <div className="space-y-1">
          <h1 className="text-4xl font-bold tracking-tight text-foreground">
            Nabilah Abas
          </h1>

          <p className="text-base text-zinc-500 font-mono">
            @nabilaanriz
          </p>
        </div>
      </div>

      {/* Introduction */}
      <div className="mb-10 min-h-[100px]">

        <p
          className={`
            text-xl
            font-medium
            mb-1
            ${
              theme === 'dark'
                ? 'text-zinc-400'
                : 'text-zinc-600'
            }
          `}
        >
          Just a chill girl passionate about
        </p>

        {/* Typing Animation */}
        <div className="flex items-center gap-2">

          <h2 className="text-4xl font-black text-foreground tracking-tighter">
            {displayText}
          </h2>

          {/* Cursor */}
          <motion.div
            animate={{
              opacity: [1, 0, 1],
            }}
            transition={{
              duration: 0.8,
              repeat: Infinity,
            }}
            className="w-1 h-10 bg-brand self-end mb-1"
          />

        </div>
      </div>

      {/* Buttons */}
      <div className="flex flex-wrap gap-4">

        {/* Resume */}
        <a
          href="/images/resume.pdf"
          target="_blank"
          rel="noopener noreferrer"
          className={`
            px-6
            py-3
            font-bold
            rounded-xl
            flex
            items-center
            gap-2
            transition-all
            active:scale-95
            text-sm
            cursor-pointer
            shadow-lg
            no-underline
            ${
              theme === 'dark'
                ? 'bg-white text-black hover:bg-zinc-200'
                : 'bg-black text-white hover:bg-zinc-800'
            }
          `}
        >
          <Download size={18} />

          Resume (Software Development)
        </a>

      </div>
    </div>
  );
}
