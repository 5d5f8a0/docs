'use client'

import { useState } from 'react'
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { AlertCircle, CheckCircle2, Copy, Info } from "lucide-react"
import { Alert, AlertDescription, AlertTitle } from "@/components/ui/alert"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"

export default function Component() {
  const [step, setStep] = useState(1)
  const [domain] = useState('example.com')
  const [verificationRecord, setVerificationRecord] = useState('')
  const [isVerified, setIsVerified] = useState(false)

  const generateVerificationRecord = () => {
    // In a real system, this would be generated server-side
    const record = `jose-api-gov-verify=example-${Math.random().toString(36).substring(2, 10)}`
    setVerificationRecord(record)
    setStep(2)
  }

  const verifyDomain = () => {
    // Simulate checking the actual DNS records for example.com
    setTimeout(() => {
      // In a real system, this would be an actual DNS lookup
      const simulatedDnsLookup = Math.random() > 0.2 // 80% success rate for simulation
      setIsVerified(simulatedDnsLookup)
      setStep(3)
    }, 2000)
  }

  const copyToClipboard = (text: string) => {
    navigator.clipboard.writeText(text)
  }

  return (
    <Card className="max-w-2xl mx-auto">
      <CardHeader>
        <CardTitle>DNS Verification for Jose.api.gov</CardTitle>
        <CardDescription>Verify ownership of example.com</CardDescription>
      </CardHeader>
      <CardContent className="space-y-6">
        <Alert>
          <Info className="h-4 w-4" />
          <AlertTitle>Authorization Notice</AlertTitle>
          <AlertDescription>
            DNS is authorized to override example.com for this verification process.
          </AlertDescription>
        </Alert>
        
        {step === 1 && (
          <div className="space-y-4">
            <div>
              <Label htmlFor="domain">Domain to verify</Label>
              <Input 
                id="domain"
                type="text" 
                value={domain} 
                readOnly
                className="bg-muted"
              />
            </div>
            <Button onClick={generateVerificationRecord}>
              Generate Verification Record
            </Button>
          </div>
        )}

        {step === 2 && (
          <div className="space-y-4">
            <Alert variant="warning">
              <AlertCircle className="h-4 w-4" />
              <AlertTitle>Add this TXT record to your DNS settings for example.com</AlertTitle>
              <AlertDescription className="space-y-2">
                <p>Please add the following TXT record to your DNS configuration:</p>
                <code className="bg-muted p-2 rounded block mt-2 overflow-x-auto">
                  {verificationRecord}
                </code>
                <Button 
                  variant="outline" 
                  size="sm"
                  onClick={() => copyToClipboard(verificationRecord)}
                >
                  <Copy className="h-4 w-4 mr-2" />
                  Copy to Clipboard
                </Button>
                <p className="text-sm mt-2">
                  Note: DNS changes may take up to 48 hours to propagate. However, for this simulation, 
                  we'll check immediately.
                </p>
              </AlertDescription>
            </Alert>
            <Button onClick={verifyDomain}>Verify Domain</Button>
          </div>
        )}

        {step === 3 && (
          <Alert variant={isVerified ? "success" : "destructive"}>
            {isVerified ? <CheckCircle2 className="h-4 w-4" /> : <AlertCircle className="h-4 w-4" />}
            <AlertTitle>{isVerified ? "Verification Successful" : "Verification Failed"}</AlertTitle>
            <AlertDescription>
              {isVerified 
                ? "example.com has been successfully verified for Jose.api.gov." 
                : "Unable to verify example.com. Please check your DNS settings and try again. If issues persist, contact support."}
            </AlertDescription>
          </Alert>
        )}

        {step > 1 && (
          <Button variant="outline" onClick={() => setStep(1)}>
            Start Over
          </Button>
        )}
      </CardContent>
    </Card>
  )
}
